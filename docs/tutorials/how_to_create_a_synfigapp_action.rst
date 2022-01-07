How to create a new synfigapp Action
====================================

This is a tutorial based on what I did to solve issue `#2005 <https://github.com/synfig/synfig/issues/2005>`_.
This feature request is about the possibility of moving the origin point of a region/outline/advanced outline layer to its geometric center.
This document will describe how to create this feature and how to make Synfig Studio able to show it to user.

As the key point on the feature request is a modification of a synfig layer parameter value, it should be done via a (new) ``synfigapp::Action``.

About synfigapp
---------------

First things first: what is ``synfigapp``? It is an intermediate API to 'interactively' handle a synfig animation/document.
It deals with stuff like layer creation, layer up/down movement (z-depth), layer parameter setting, action history, layer selection, canvas resizing, etc.
In other words, 'everything' that should be done/changed in a Synfig animation document. This kind of stuff is called Action. And there are so many of them, more than 100!
``synfigapp`` is also responsible to emit some signals at each action. Therefore, it is a 'perfect' glue between the synfig document and an interactive application that uses or creates it.

Synfig Studio is a GUI built on top of synfigapp and should not change a synfig file directly.
For example, when a user renames a layer in Layers Panel, the GUI calls an synfigapp action to rename it instead of using the synfig library directly.
One important reason to do it via synfigapp is the handling of action history (made by synfigapp), that allows user undo the last action(s).

How does Synfig Studio work in this regard?
-------------------------------------------

When user right-clicks on a layer visual handle (like tangent, origin, vertex, etc.), the ``Workarea`` class detects the mouse button pressing event,
realizes that it was over a handle (Synfig Studio internally calls it 'duck') and dispatches the event to the duck object.
The event detection is done in ``bool studio::WorkArea::on_drawing_area_event(GdkEvent *event)`` and the dispatch is via ``void studio::Duck::signal_user_click(int button_num)``.

Each handle (duck) type may react to this signal calling differently, but it is pretty common to they call ``studio::CanvasView::popup_param_menu()`` and pass,
as method argument, what layer parameter they affect.
In its turn, it calls ``studio::CanvasView::make_param_menu()`` that, in short, scans all synfigapp actions and ask each one if it is a candidate to deal with that layer parameter/value node.

At this point, Studio pops up a context menu with all related actions (and some other special stuff).
When user selects a menu item, ``void Instance::process_action(synfig::String action_name, synfigapp::Action::ParamList param_list)`` is called,
where it may ask user for some confirmation or for extra info and then check if the action tells it is ready. Finally, the action is performed.

In conclusion, the new ``synfigapp::Action`` must be able to provide what data (layers, parameters/valuenodes, etc.) it expects, to provide a name/label to be used in GUI, to check if it is a candidate for current user context, and to do (and hopefully undo) the action itself.

synfigapp Actions
-----------------

As we now know what any synfigapp action should do, let us explore how they are structured in synfigapp.

There are 4 types of Action:

* synfigapp::Action::Base – obviously the base for any action
* synfigapp::Action::Undoable (derived from synfigapp::Action::Base) – adds the ``undo()`` method
* synfigapp::Action::Super (derived from synfigapp::Action::Undoable and synfigapp::Action::CanvasSpecific) – it is, actually, a sequence of other actions
* synfigapp::Action::Group (derived from synfigapp::Action::Super) – ?

synfigapp::Action::CanvasSpecific is an 'interface' to provide basic and standard support to those actions that must be used on a specific Canvas. Any action that modifies layers, valuenodes, etc. would inherit this class. And it is very important to inherit synfigapp::Action::Undoable, because it is the way synfigapp knows the action is… undoable. Thus, many actions explicitly inherit both classes, if they aren't already based on synfigapp::Action::Super.

Here is the mandatory methods to be implemented for 'any' Action (defined in ``synfigapp/action.h``):

==========  ===========================================================  =========================
Base Class  Method                                                       Description
==========  ===========================================================  =========================
Base        virtual synfig::String get_name() const;                     The internal action name. Must be unique.
                                                                         Used for creating action instances by name (``synfigapp::Action::create(const synfig::String& name)``).

                                                                         The recommended way to declare and implement it is via ACTION_ macros listed below.

Base        static Action::Base* create();                               Factory for creating this action later configured via ``set_param()`` if needed.

                                                                         The recommended way to declare and implement it is via ACTION_ macros listed below.

Base        static bool is_candidate(const ParamList &x);                Checks the ParamList to see if this action could be performed with given parameters.

Base        static ParamVocab get_param_vocab();                         Yields the ParamVocab object which describes what parameters this action needs before
                                                                         it can perform the act.

Base        bool set_param(const synfig::String& name, const Param& v);  The action implementation receives the value ``v`` for parameter ``name`` by this method.

Base        virtual bool is_ready() const;                               Check if the current parameter list set for action is enough and
                                                                         it would be ok to call ``perform()``

Base        virtual void perform();                                      As name says, this method does what action is supposed to do.

                                                                         It must throw an ``Action::Error`` on failure

Undoable    virtual void undo();                                         If this action class is derived from synfigapp::Action::Undoable,
                                                                         it must implement here how to undo it.

Super       virtual void prepare();                                      This method is responsible to make the sequence of actions to be performed.

                                                                         From the parameters set by e.g. ``set_param()``, it will create the needed actions
									 that will compound the action sequence.
==========  ===========================================================  =========================

Helper macros (provided by ``synfigapp/action.h``):

.. code-block:: cpp

  // Inside class declaration
  ACTION_MODULE_EXT
  
  // In class implementation file (example: class Action::ActivepointSet)
  ACTION_INIT(Action::ActivepointSet)
  ACTION_SET_NAME(Action::ActivepointSet,"ActivepointSet");
  ACTION_SET_LOCAL_NAME(Action::ActivepointSet,N_("Set Activepoint"));
  ACTION_SET_TASK(Action::ActivepointSet,"set");
  ACTION_SET_CATEGORY(Action::ActivepointSet,Action::CATEGORY_ACTIVEPOINT);
  ACTION_SET_PRIORITY(Action::ActivepointSet,0);
  ACTION_SET_VERSION(Action::ActivepointSet,"0.0");

Solving the issue
-----------------

The logic for the new action
............................

The new action purpose is to move the origin point of a region/outline/advanced outline layer to its geometric center.
Let us assume the layer geometric center is the center of its (AABB) bounding rectangle (that one shown on Synfig Studio when you select the layer).
It may be a problem for convex or concave shapes. However, for simplicity sake of this documentation, I will adopt this concept.

Synfig Studio let user create geometric shapes by its tools Circle, Rectangle, Star, Polygon and Spline. Internally they use Layer_Circle, Layer_Polygon, Rectangle, Star, Region, Outline and Advanced_Outline layers. All of them are (directly or indirectly) derived from Layer_Shape class.
From this list, Layer_Circle and Star always have their origin point set at their geometric center. Layer_Ractangle does not have an origin parameter. So the focus is the layers Layer_Polygon, Region, Outline and Advanced_Outline.

Layer_Shape has the convenient method ``Rect Layer_Shape::get_bounding_rect()``. So deriving the geometric center of those layers is pretty easy.

The new action parameters
.........................

The action shall receive the origin parameter that should be displaced. From this information, it can retrieve the layer it belongs to and access the mentioned method. Once computed the new origin position, it should set the origin parameter value.

However, if the origin has a new value set, all vertices will rendered in a new place, as their coordinates are relative to origin point. Therefore it is necessary to change (by a constant offset) the coordinates of all vertices. Layer_Poligon stores them in 'vector_list' parameter; the others in 'bline' parameter.

The new action base class
.........................

The action is specific to a canvas (the one the layer is on), so it must be derived from auxiliar class ``synfigapp::Action::CanvasSpecific``.

It is obvious the action can be undoable, so ``synfigapp::Action::Undoable`` must be a base class too for it.

Obviously, synfigapp already has an action for setting a layer parameter. It is called ValueDescSet. We could reuse it, as we have to change a lot of parameters, instead of repeting code. That would also avoid we having to handle the undo method, as ValueDescSet already know to undo.

If we are reusing an existent action (and several times: origin + vertices), we should use as our base class for action the ``synfig::Action::Super`` class only, as it is derived from ``synfigapp::Action::Undoable`` and ``synfigapp::Action::CanvasSpecific`` and the main reason: it handles sequence of existent actions.

Creating the new action class and its files
...........................................

The new class will be named as OriginToShapeCenter, following the action naming convention: `camel case <https://en.wikipedia.org/wiki/Camel_case>`_. I chose OriginToShapeCenter because OriginToCenter is too much generic: it could mean canvas center, for example.

The filenames will be ``origintoshapecenter.h`` and ``origintoshapecenter.cpp``, as the convention used for synfigapp action files are the action name in snake_case, but (ugh) without the underscore ``_``. They will be placed under ``synfig-studio/src/synfigapp/actions`` folder.

The new files must be registered in the building systems. As we support Autotools and CMake, we have to edit the following files:

* ``synfig-studio/src/synfigapp/Makefile.am``
* ``synfig-studio/src/synfigapp/actions/CMakeLists.txt``

We have to edit an extra file too, related to i18n: ``synfig-studio/po/POTFILES.in``.

Note: Don't forget to regenerate the Makefiles due to this change. If you are using the building scripts provided by synfig project, run ``./2-build-production.sh studio build`` once. It is only required because we changed a Makefile.am and/or CMakeLists.txt.


DRAFT
.....

