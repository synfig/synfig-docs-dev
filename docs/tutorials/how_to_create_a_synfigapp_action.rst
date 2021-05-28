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
* synfigapp::Action::Super (derived from synfigapp::Action::Base and synfigapp::Action::CanvasSpecific) – it is, actually, a sequence of other actions
* synfigapp::Action::Group (derived from synfigapp::Action::Super) – ?

synfigapp::Action::CanvasSpecific is a 'interface' to provide basic and standard support to those actions that must be used on a specific Canvas. Any action that modifies layers, valuenodes, etc. would inherit this class. And it is very important to inherit synfigapp::Action::Undoable, because it is the way synfigapp knows the action is… undoable. Thus, many actions explicitly inherit both classes, if they aren't already based on synfigapp::Action::Super.

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

One of the first things to do is to decide what is the base class for our new Action:


Now let's create the new files ``origintocenter.h`` and ``origintocenter.cpp``: they will be placed under ``synfig-studio/src/synfigapp/actions`` folder.

Next I registered in the building system files. As we support Autotools and CMake, we have to edit:

* ``synfig-studio/src/synfigapp/Makefile.am``
* ``synfig-studio/src/synfigapp/actions/CMakeLists.txt``

And register the action in ``synfigapp::Action`` book. For now, the only way is directly in ``syfigapp::Action::Main()`` method (in file ``synfig-studio/src/synfigapp/action.cpp``).

Next I'll start to implement the new action class. The essencial methods are shown below:

