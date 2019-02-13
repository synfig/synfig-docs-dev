.. _building:

Code structure
===============


Overview
--------

Synfig is divided into three components: ETL, synfig-core and synfig-studio.

* **ETL** is a template library that implements reference counting, portable threading and other low-level stuff. Every part of the Synfig project uses ETL in some way. It is like the C++ STL.
* **synfig-core** is Synfig's backend. It renders scenes and knows how to read and write Synfig XML files. This directory contains the Synfig library and the Synfig command-line tool. 
* **synfig-studio** is the graphical editor. It uses the GTK+ widget library. If you want to hack on the interface, this is what you should look at.

The structure of **synfig-core** is:

* ``synfig-core/src/synfig/`` - Code of **"libsynfig"** library. This is actually the main part of synfig-core. It contains code of render engine and routines for reading/writing Synfig's files. The **libsynfig** library is used by all other Synfig's components.
* ``synfig-core/src/modules/`` - Functionality of **libsynfig** can be extended with modules and this directory is a place for them. A module can do following things:
   * Add support for importing specific file format(s). Examples:
      * ``synfig-core/src/modules/mod_png/mptr_png.cpp``
      * ``synfig-core/src/modules/mod_bmp/mptr_bmp.cpp``
   * Add support for exporting (rendering) to specific file format(s). Examples:
      * ``synfig-core/src/modules/mod_png/trgt_png.cpp``
      * ``synfig-core/src/modules/mod_gif/trgt_gif.cpp``
   * Implement a layer type(s). Examples:
      *  ``synfig-core/src/modules/mod_geometry/circle.cpp`` - Circle Layer
      *  ``synfig-core/src/modules/lyr_freetype/lyr_freetype.cpp`` - Text Layer
      *  ``synfig-core/src/modules/mod_noise/distort.cpp`` - Noise Distort Layer	 
   * Implement a **valuenode** (see below on valuenodes). Example:
      * ``synfig-core/src/modules/mod_noise/valuenode_random.cpp``
* ``synfig-core/src/tool/`` - Code of synfig command-line tool (binary is simply called ``synfig``). It uses **libsynfig** to read Synfig files files and render them in any supported format.

Main components of **synfig-studio**:

- ``synfig-studio/src/synfigapp/`` - Code of **libsynfigapp** library. This is a layer between GUI and **libsynfig** (from synfig-core). It contains code for **actions** - operations that transform loaded Synfig's file in some way. When user makes some change to Synfig file in GUI, then an *action* is called that does the actual modification.
- ``synfig-studio/src/gui/`` - Code of GUI written in GTKmm. This defines how application looks and behave.



So, we have following structure:

::

  gui ---> libsynfigapp -------> libsynfig
  
             synfig-cli -------> libsynfig


How Synfig works
----------------

Let's take a few moments to understand the main concepts.

The basic building block in Synfig is **Layer**.

.. note::

   ``synfig-core/src/synfig/layer.cpp``

Layer either displays some graphical information on the screen (circle, line, text, etc.), or does some transformation of graphic information *under* it - i.e. acts as **filter** (blur, translation, noise distortion, etc.).

.. note::

   Built-in layers:
     - ``synfig-core/src/synfig/layers/``
   
   Layers as modules: 
     - ``synfig-core/src/modules/lyr_std``
     - ``synfig-core/src/modules/lyr_freetype``
     - ``synfig-core/src/modules/mod_example``
     - ``synfig-core/src/modules/mod_geometry``
     - ``synfig-core/src/modules/mod_gradient``
     - ``synfig-core/src/modules/mod_noise``
     - ``synfig-core/src/modules/mod_particle``
     

Layers are placed in particular order. 

::

  - Layer 3
  - Layer 2
  - Layer 1

Layers are rendered from bottom to top.

All graphic information under particular layer is called its **Context**.

In example above ``Layer 2`` together with ``Layer 1`` are context of ``Layer 3``.

If ``Layer 3`` is a graphic layer, then it displays some graphic information, which is composed with its context (``Layer 1`` and ``Layer 2``). The way how ``Layer 3`` composed with its context is defined by layer's **Blend Method**.

.. note::

   ``synfig-core/src/synfig/color/``
   
   TODO: Add link to list of available blend methods

If ``Layer 3`` is a filter layer, then it transforms graphic information created by ``Layer 1`` and ``Layer 2``.

A structure which stores a list of layers is called **Canvas**.

.. note::

   ``synfig-core/src/synfig/canvas.cpp``

A canvas can also store other canvases. This is how layers organized into hierarchy.

In GUI canvases represented by Groups, but in code they are called Paste Canvases. Past Canvas is a special type of layers, which holds Canvas of other layers.

.. note::

   ``synfig-core/src/synfig/layers/layer_pastecanvas.cpp``

Every Synfig file has a Root Canvas, which contains all layers. Also it can have several *Exported* canvases - a separate canvases that are outside of Root Canvas.

Paste Canvas can be **inline** (i.e. include all its content in itself) or **linked** (i.e. reference content from exported canvases or other Synfig files).

.. note::

   Loading Synfig file:  ``synfig-core/src/synfig/loadcanvas.cpp``
   
   Saving Synfig file: ``synfig-core/src/synfig/savecanvas.cpp``

Every layer has a set of **Parameters**, which define how layer is rendered (and *what* it is rendering).

In simplest case layer parameter can be defined by a value of particular type - **ValueBase** (Integer, Real, Bool, Color, etc).

.. note::

   ``synfig-core/src/synfig/base_types.cpp``
   
In complex case parameter can be defined by **ValueNode**. ValueNode is a formula that produces a value from some calculations. Each ValueNode has parameters that define input data for formula. Parameters of ValueNode can also be represented by ValueBases or ValueNodes, so it is possible to construct nested formulas.

.. note::

   ``synfig-core/src/synfig/valuenodes/``
   ``synfig-core/src/modules/mod_noise/valuenode_random.cpp``

It is possible to link ValueNodes and ValueBases for different layers.

TODO: Make an illustration of layers sharing same ValueNodes/ValueBases



To be continued...

..
	TODO: Include info from https://wiki.synfig.org/Dev:How_Synfig_Works

	TODO: Write about linking

	TODO: Write about animated ValueNodes.

	TODO: Write about rendering engines.
  synfigapp
  ---------

	**main**

	../synfigapp/main - stores information for the entire application (fg/bg colors, width, settings, input devices)
	 
	../synfigapp/instance - information unique for each instance (root canvas, canvas interface list, selection manager, save/save_as)
	 
	../synfigapp/canvas_interface - information unique to each exported canvas (I believe opening a canvas in the canvas browser loads a new interface, but not a new instance)
	;* current time (at playhead), editing mode (normal/animated)
	;* wrappers for various actions, such as adding layers or adding/setting/converting valuenodes
	 
	../synfigapp/value_desc - link to a value node (eg. layer.param_name parent_value_node.param_index; animated.waypoint; canvas.param)
	valuelink - (?) Valuebase link. Inherits from synfig-core, why is this in studio/gtkmm?
	 
	../synfigapp/inputdevice - input devices
	../synfigapp/settings - settings
	../synfigapp/selectionmanager (look-into) - selection manager interface, null selection manager
	../synfigapp/editmode - edit mode (normal, animated)
	../synfigapp/uimanager - interface class for a UI interface (Dialogs such as yes_no, yes_no_cancel, etc) The actual UI interface used is defined elsewhere

	**action system**

	../synfigapp/action - defines types of actions: action, undoable, canvasspecific, super, group
	../synfigapp/action_param - defines parameters for action
	../synfigapp/action_system - action system and passive grouper
	../synfigapp/actions/* - individual action

	**misc**

	../synfigapp/general.h, general.h - gettext macros
	../synfigapp/cvs - cvs system
	../synfigapp/timegather - (?)

	gui
	---------

	[Core UI]
	main - entry point, creates an instance of App
	app - initializes the application (loads all UI components)
	;* manages instances (which one is selected), canvas views, preferences
	autorecover - automatic recovery (references app, uses instance)
	devicetracker: save/load preferences and init extended input devices
	instance - (?) inherits from synfigapp::Instance
	 
	[Misc UI]
	splash - splash screen window
	about - about dialog
	adjust_window - (?) Adjustment Window, uses scale factor
	onemoment - window saying "one moment, please"
	dialog_setup
	widget_filename
	iconcontroller - pairs icon files with gtk names. Can get an icon for a valuenode or layer
	 
	[Canvas view]
	canvasview - makes the menus; receives on_duck_changed events; creates a workarea
	 
	framedial - a table with play/forward/backward buttons
	keyframedial - buttons for seek next, previous, lock
	resolutiondial - Increase/decrease/ use low res
	toggleducksdial - Show/hide various types of ducks
	zoomdial - zoom in/out/etc
	 
	[================== Ducks and Tools =================]
	 
	[Workarea]
	workarea - [inherits from duckmatic and Gtk::Table] the workarea
	event_layerclick - event for layer clicked
	event_mouse - stores the mouse button pressed and any modifier keys
	eventkey - key of events (e.g. refresh, stop, undo, workarea clicked...)
	 
	[Ducks]
	duckmatic - manages ducks, ducks_dragger, strokes, and Beziers
	;* (Also defines duckdrag_base and translate)
	;* When a duck drag is done, passes the new locations of the duck to canvasview (reverse manipulation function)
	 
	duck - a duck (stores either a point or an angle of rotation)
	ducktransform_* - define duck transformations. These are used to transform the ducks so they line up with a transformed object on-screen
	 
	[Toolbox]
	toolbox.h - the toolbox
	widget_defaults - the fill/outline/etc selection widget in the toolbox
	widget_tooloptions
	 
	[State system]
	smach.h - typedef etl::smach<CanvasView,EventKey> Smach; // [state machine]
	statemanager -keep track of states
	state_* - all of the states
	;* states such as normal and rotate define their own duck draggers
	../synfigapp/blineconvert - used by draw tool to convert list of points to a bline
	 
	[================ Docks and Dialogs =================]
	[Docks System]
	dockmanager - gets size, position, or contents of a dockable, registers/unregisters dockables, find dockable or DockDialog, present a given dockable (takes a name)
	 
	dockable - generic class for dockables. "dnd" is "drag-and-drop"
	dockbook - a notebook (tabbed group) of docks
	dockdialog - a window, presumably  containing various dockbooks (tabbed groups) of dockables
	dock_canvasspecific - base class for canvas-specific dockables
	 
	[Docks]
	dock_info - (shows mouse position and the color under it)
	dock_navigator
	dock_history
	 
	dock_curves - uses curves widget + some time sliders
	widget_curves
	 
	[Tree docks]
	canvastreestore- (?)
	 
	dock_canvases - canvas browser
	 
	dock_timetrack
	widget_timeslider - the time track, labeled at regular intervals
	dialog_keyframe
	dialog_waypoint
	widget_keyframe_list
	widget_waypoint
	widget_waypointmodel
	keyframeactionmanager - "Add new keyframe" and "keyframe properties" buttons, keeps track of keyframe tree
	keyframetree - TreeView of keyframes
	keyframetreestore - stores keyframes (is there any point to keyframe_tree_store_class_?)
	 
	dock_metadata
	metadatatreestore - model for metadata tree
	 
	dock_layergroups
	layergrouptree - TreeView of layer groups
	layergrouptreestore - model for layer group tree
	 
	dock_children
	childrentree - TreeView of canvas' children
	childrentreestore - model for children tree
	 
	dock_layers
	dock_params
	layerparamtreestore - model for layer params tree
	layertreestore - model for layers tree
	layertree - returns TreeViews of layers and params
	layeractionmanager - keeps track of layer tree; creates actions relating to layers
	 
	[Widgets for valuenodes]
	widget_value - picks the right widget for a valuenode
	 
	widget_canvaschooser - Canvas valuenode (select canvas)
	widget_color
	widget_coloredit
	widget_gradient - gradient valuenode
	widget_compselect - select the composition (file) being edited
	widget_distance - spinbutton (for type real when it's a distance)
	widget_enum - enum type values
	widget_time - time valuenode
	widget_vector - (aka point)
	 
	[Dialogs]
	dialog_color - select a color
	dialog_gradient -set a gradient
	canvasoptions -toggles grid snapping, visibility, and size
	canvasproperties - name, id, info, and metadata
	 
	[======================= Other ======================]
	 
	[Renderer system] - I have not looked into this much
	asyncrenderer
	preview - Preview class and the preview widget
	renddesc - RendDesc widget (Render menu - why is it called desc?)
	renderer_* - rendering system
	workarearenderer
	dialog_preview
	dialog_targetparam - parameters for rendering target
	 
	[Audio system] - Did not look at, as it is disabled
	audiocontainer
	dialog_soundselect
	widget_sound
	 
	[Modules]
	./mod_mirror/ - Mirror tool
	./mod_palette/ - Palette
	module - interface class for models: has methods start() stop()
	 
	[======================= MISC =======================]
	 
	ipc - (?)
	keymapsettings - (Defines the structures for managing key map settings) affects accelerators
	 
	groupactionmanager - (look-into) references LayerGroupTree
	 
	compview - Does not appear to be used anywhere
