.. _building:

Code structure
===============

Things to know before you start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Synfig is divided into three components: ETL, synfig-core and synfig-studio.

* ETL is a template library that implements reference counting, portable threading, gaussian blur, and plenty of other goodies. Every part of the Synfig project uses ETL in some way. It is like the C++ STL.
* synfig-core is Synfig's backend. It renders scenes and knows how to read and write Synfig XML files. This directory contains the Synfig library and the Synfig command-line tool. 
* synfig-studio is the graphical editor. It uses the GTK+ widget library. If you want to hack on the interface, this is what you should look at.

synfig-core
~~~~~~~~~~~~~~~~~~~~~~

The code for synfig-core is located in "synfig-core/src". There you will find 3 important directories:

* "synfig" - a code of "libsynfig" library. It contains code of rendering engine and used by all other Synfig's components. Render engine can be extended with modules (see below).
* "modules" - modules for rendering engine. There are two types of modules:
  * "layers" - allow to add new layer types
  * "targets" - providing support for import/export to various formats.
* "tool" - a code of synfig commandline tool (binary is simply called "synfig"). It uses libsynfig to read Synfig files files and render them in any supported format.

Since with "modules" and "tool" everything pretty much trivial, let's dive into "synfig" - a libsynfig structure.

...

synfig-studio
~~~~~~~~~~~~~~~~~~~~~~

The code for synfig-core is located in "synfig-core/src". There you will find 3 important directories:

* "synfigapp" - ...
* "gui" - ...
* "brushlib" - ...


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
