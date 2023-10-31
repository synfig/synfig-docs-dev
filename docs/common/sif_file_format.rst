.. toctree::
 :maxdepth: 2

.. _structure:

Synfig File Format (.sif)
=========================


Overview
--------

Synfig store its data in a `XML <https://en.wikipedia.org/wiki/XML>`_ file format but with a ``.sif`` file extension.

Current stable versions of Synfig Studio have `.sifz` file extension set as default when saving a new animation file. It is just a ``.sif`` file compressed
by using `gzip <https://www.gzip.org/>`_ file format.

This document describes the `.sif` version 1.2 (used in Synfig Studio >= 1.4).

Since version 1.0, all real numbers must use dot character as the decimal separator.

.. _canvas:

The root node and painting world: 'canvas'
------------------------------------------

Overview
~~~~~~~~

The 'canvas' is the only possible root node element in this XML file, but it may be used as child node too.

As a child node, it can be described inline, be part of another canvas 'library' ('defs' node) or even may refer to an external file. See 'id' attribute.

The <canvas> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

'canvas' may have any of these elements in any order*:

* authoring info: `'name'`_, `'desc'`_, `'author'`_
* other meta info: `'meta'`_
* library info: `'defs'`_, `'bones'`_, `'keyframe'`_
* content info: `'layer'`_

Attributes
^^^^^^^^^^

:id: <optional> <string> canvas identifier. Ignored if it is the root canvas.
:guid: <optional>
:version: <optional> <string> the canvas version. Current default: "1.2".
:width: <optional> <positive integer> 
:height: <optional> <positive integer> 
:xres: <optional> <positive real> 
:yres: <optional> <positive real> 
:gamma-r: <optional> <positive real> The color correction factor for red component (since 1.1)
:gamma-g: <optional> <positive real> The color correction factor for green component (since 1.1)
:gamma-b: <optional> <positive real> The color correction factor for blue component (since 1.1)
:fps: <optional> <positive real> How many frames should be rendered per second
:begin-time: <optional> <Time> 
:end-time: <optional> <Time> 
:antialias: <optional> <positive integer> The antialias amount
:view-box: <optional> <list of 4 real> The 2D coordinates of top-left point and bottom-right point of the view box, in this order, separated by a single space: tl_x tl_y br_x br_y
:bgcolor: <optional> <list of 4 real> Background color represented by red, green, blue and alpha components, separated by single space. Values in the range (0.0, 1.0).
:focus: <optional> <list of 2 real> The canvas focus point (for zooming). The point coordinates are separated by a single space.

.. _canvas_id:
A canvas 'id' may be any string without '#' and ':' characters. When a node refers to a canvas, it will use its 'id' to identify it.
This reference id has the following format:

``[[file-path]#]canvas-id[:child-canvas-id][:child-canvas-id][:child-canvas-id]…``

If ``file-path`` is empty, the refered canvas is in the current document file.

The `canvas-id` is the ID of a canvas of the selected document file. If empty, it means the root canvas of the file.

The `child-canvas-id` is the ID of the previously listed canvas.


'name'
------------------------------------------

Overview
~~~~~~~~

A name for the canvas, like a title.

It only makes sense one of this node per canvas.

The <name> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any character data.

'desc'
------------------------------------------

Overview
~~~~~~~~

The canvas description.

It only makes sense one of this node per canvas.

The <desc> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any character data.

'author'
------------------------------------------

Overview
~~~~~~~~

The canvas author.

It only makes sense one of this node per canvas.

The <author> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any character data.

'meta'
------------------------------------------

Overview
~~~~~~~~

It is used to store some meta information, usually for some Synfig Studio settings, specific to a file. For example: 'grid_show', 'grid_size', 'grid_snap'.
It shouldn't affect final rendering.

It only applies to non-inline canvas (i.e. any group layer cannot have this kind of child node).

The <meta> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Empty

Attributes
^^^^^^^^^^

:name: <string> The meta info name/identifier.
:content: <string> The value related to this meta info.

'defs'
------------------------------------------

Overview
~~~~~~~~

Canvas may have an internal library/collection of canvases and value nodes, each one of them with an unique id. They are called 'exported canvas' or 'exported value node', respectively. Any internal canvas or valuenode that has an ID assign to it is inserted in this Canvas Library automatically, and they are stored here.

The Canvas Library components are defined here, and can be referred in layer or value node parameters by their ID by using the node attribute 'use_'.

'defs' only applies to non-inline canvas (i.e. any group layer cannot have this kind of child node).

The <defs> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of 'canvas_' elements or `Value Node elements`_ in any order. All of them must have their 'id' attribute set.

Attributes
^^^^^^^^^^

None

'bones'
------------------------------------------

Overview
~~~~~~~~

Any bone a Canvas uses is registered here, each one of them with an unique id.

'bones' only applies to non-inline canvas (i.e. any group layer cannot have this kind of child node).

The <bones> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of 'bone' elements and a mandatory (and single) 'bone_root' in any order. All of them must have their 'guid' attribute set.

Attributes
^^^^^^^^^^

None

'keyframe'
------------------------------------------

Overview
~~~~~~~~

It defines a time point mark in the animation. A canvas can have any number of keyframes. 

It only applies to non-inline canvas (i.e. any group layer cannot have this kind of child node).

The <keyframe> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any character data. It is a metadata: an optional keyframe description.

Attributes
^^^^^^^^^^

:time: <Time>
:active: <optional> <boolean> Default: true.

'layer'
------------------------------------------

Overview
~~~~~~~~

Every canvas is composed of layers. Layers can draw stuff on a canvas (e.g. polygons), apply some filter or effects (e.g. shadows, blur, etc.) or some other manipulations (e.g. time loop, sound, etc.).

Layers are rendered/handled from bottom to top, and may use a tree structure, if Layers of "PasteCanvas" type are used (like "Group" layer). So the order of 'layer' elements in a 'canvas' node matters.

The <layer> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

'layer' may have any number of these elements, and the order matters (see `'layer'`_ Overview):

* layer parameter: `'param'`_

Attributes
^^^^^^^^^^

:type: <string> The layer type. In source code, it is also known as layer name. Not localized.
:group: <optional> <string> If present, it says which Layer Set the element belongs to. Do not confuse with a Layer_PasteCanvas subclass 'Group'.
:version: <optional> <string> A string (usually in the `semantic versioning <https://semver.org/>`_) that represents the layer implementation version
:desc: <optional> <string> Layer description. GUI uses it as layer label.
:active: <optional> <boolean> If layer will be 'rendered'/'applied' in an visual editor. See ``exclude_from_rendering``. Default: true.
:exclude_from_rendering: <optional> <boolean> If layer will be rendered/applied. See ``active``. Default: false.

'param'
------------------------------------------

Overview
~~~~~~~~

Some (many) layers support some set up via parameter.

A parameter may be filled with a fixed value or with a dynamically computed value (aka. ValueNode).

The <param> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

'param' may have at most one of these elements:

* a value: `'real'`_, `'time'`_, `'integer'`_, `'string'`_, `'vector'`_, `'color'`_, `'segment'`_, `'list'`_, `'gradient'`_, `'bool'`_, `'angle'`_, `'bline_point'`_, `'guid'`_, `'width_point'`_, `'dash_item'`_, `'transformation'`_, 'canvas_', `'weighted_{type}'`_, `'pair_{type1}_{type2}'`_.  See `Value elements`_.

* a value node: `'animated'`_, `'static_list'`_, `'dynamic_list'`_, `'bline'`_, `'wplist'`_, `'dilist'`_, `'weighted_average'`_, `'average'`_, 'canvas_', any registered linkable value node, any `Value elements`_. See `Value Node elements`_.

Attributes
^^^^^^^^^^

:name: <string> The layer parameter name. Not localized.
.. _use:
:use: <optional> <string> If present, it says that the parameter is defined by an exported canvas (if parameter value type is Canvas) or valuenode otherwise (see `'defs'`_). Currently, no child node is allowed when a parameter refers to exported data.
:guid: <optional> <string>


Value elements
------------------------------------------

Overview
~~~~~~~~

A value element may be of different types. Current possible nodes are: `'real'`_, `'time'`_, `'integer'`_, `'string'`_, `'vector'`_, `'color'`_, `'segment'`_, `'list'`_, `'gradient'`_, `'bool'`_, `'angle'`_, `'bline_point'`_, `'guid'`_, `'width_point'`_, `'dash_item'`_, `'transformation'`_, 'canvas', `'weighted_{type}'`_, `'pair_{type1}_{type2}'`_.

.. _Common_Value_Attributes:

Common Attributes
^^^^^^^^^^^^^^^^^

:static: <optional> <boolean> If value may be converted to a valuenode, or it is a constant value. Default: false.
:interpolation: <optional> <string> the default interpolation type for this value (layer parameter) if it can be animated. Supported values: "halt", "constant", "linear", "manual", "auto" (TCB), "clamped". Default: "undefined".
:guid: <optional> <string> If this attribute is present, it is actually a simplified value node Const or a link to another (non-exported) value node.

'real'
------------------------------------------

Overview
~~~~~~~~

A real number. It must use dot ('.') as decimal separator.

The <real> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <real> The value… It must use dot ('.') as decimal separator.
:ref:`Common_Value_Attributes`

'time'
------------------------------------------

Overview
~~~~~~~~

A determined point in time (or time length).

It may be expressed in seconds (as a real value), in a hour-minute-second-frames string, in frames (as an integer followed by 'f') or special strings "SOT" or "EOT".

The <time> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <string> A real value expressing time in seconds; or a string in HH:MM:SS.FF format; or an integer followed by 'f' representing how many frames; or special string "SOT" or "BOT", meaning "start of time" and "EOT" for "end of time".
:ref:`Common_Value_Attributes`

'integer'
------------------------------------------

Overview
~~~~~~~~

An integer number.

The <integer> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <integer> The value…
:ref:`Common_Value_Attributes`

'string'
------------------------------------------

Overview
~~~~~~~~

Any character sequence.

The <string> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any character data

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'vector'
------------------------------------------

Overview
~~~~~~~~

A 2D point or direction.

The <vector> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'x', 'y' (with real contents).

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'color'
------------------------------------------

Overview
~~~~~~~~

Color definition by RGBA components in the range (0.0, 1.0)

The <color> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'r', 'g', 'b', 'a' (with real contents).

Attributes
^^^^^^^^^^
:pos: <real> Used only (and required) when it is a child node of `'gradient'`_. It should be in the range (0.0, 1.0).
:ref:`Common_Value_Attributes`

'segment'
------------------------------------------

Overview
~~~~~~~~

It describes a line/curve segment by its start and end points ('p1' and 'p2', respectively) and the tangents at these points ('t1' and 't2').

The <segment> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'p1', 't1', 'p2', 't2'.

Those elements require one single child: `'vector'`_.

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'list'
------------------------------------------

Overview
~~~~~~~~

It is just a list of values.

The <list> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `Value elements`_

Attributes
^^^^^^^^^^

None

'gradient'
------------------------------------------

Overview
~~~~~~~~

It describes a line/curve segment by its start and end points ('p1' and 'p2', respectively) and the tangents at these points ('t1' and 't2').

The <gradient> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'color'`_ with 'pos' attribute.

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'bool'
------------------------------------------

Overview
~~~~~~~~

An boolean value.

The <bool> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <boolean> "true" or "false".
:ref:`Common_Value_Attributes`

'angle'
------------------------------------------

Overview
~~~~~~~~

It represents an angle in degrees.

The <angle> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <real> The value…
:ref:`Common_Value_Attributes`

'bline_point'
------------------------------------------

Overview
~~~~~~~~

It describes a control point of a spline.

The <bline_point> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'vertex', 't1', 't2', 'width', 'origin' in any order.

The first 3 elements require one single child: `'vector'`_, but 'width' and 'origin' must contain a `'real'`_ child.

't1' is the income tangent, while 't2' is the outcome tangent (with x-axis and y-axis rotated 180°).

Attributes
^^^^^^^^^^

None

'guid'
------------------------------------------

Overview
~~~~~~~~

GUID is an accronym for Globally Unique Identifier, used internally by Synfig for every node.

The <guid> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It should be empty.

Attributes
^^^^^^^^^^

:value: <string>

'width_point'
------------------------------------------

Overview
~~~~~~~~

It describes a control point of advanced outline width.

The <width_point> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'position', 'width', 'side_before', 'side_after', 'lower_bound', 'upper_bound' in any order.

'position', 'width', 'lower_bound' and 'upper_bound' must contain a single `'real'`_ element.

'side_before' and 'side_after' must contain a single `'integer'`_ element, that is, actually, an enumeration index:

==================  ===
TYPE_INTERPOLATE     0
TYPE_ROUNDED         1
TYPE_SQUARED         2
TYPE_PEAK            3
TYPE_FLAT            4
TYPE_INNER_ROUNDED   5
TYPE_INNER_PEAK      6
==================  ===

Attributes
^^^^^^^^^^

None

'dash_item'
------------------------------------------

Overview
~~~~~~~~

It describes an item of a dash pattern sequence for advanced outlines.

The <dash_item> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'offset', 'length', 'side_before', 'side_after' in any order.

'offset' and 'length' must contain a single `'real'`_ element.

'side_before' and 'side_after' must contain a single `'integer'`_ element, that is, actually, an enumeration index:

==================  ===
TYPE_INTERPOLATE     0
TYPE_ROUNDED         1
TYPE_SQUARED         2
TYPE_PEAK            3
TYPE_FLAT            4
TYPE_INNER_ROUNDED   5
TYPE_INNER_PEAK      6
==================  ===

Attributes
^^^^^^^^^^

None

'transformation'
------------------------------------------

Overview
~~~~~~~~

It describes an item of a dash pattern sequence for advanced outlines.

The <transformation> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'offset', 'scale', 'angle', 'skew_angle' in any order.

'offset' and 'scale' must contain a single `'vector'`_ element.

'angle' and 'skew_angle' must contain a single `'angle'`_ element.

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'weighted_{type}'
------------------------------------------

Overview
~~~~~~~~

This description comprehends all value types and they are named such as 'weighted_real', 'weighted_string', etc.

Any type described in `Value elements`_ is supported.

The <weighted_{type}> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'weight', 'value' in any order.

'weigth' must contain a single `'real'`_ element.

'value' must contain a single element of correspondent type (as in element name).

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

'pair_{type1}_{type2}'
------------------------------------------

Overview
~~~~~~~~

This description comprehends any combination of all value types and they are named such as 'pair_real_real', 'pair_integer_string', etc.

Any type described in `Value elements`_ is supported.

The <pair_{type1}_{type2}> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

One instance of each of these elements: 'first', 'second' in any order.

'first' and 'second' must contain a single element of its correspondent type (as in element name).

Attributes
^^^^^^^^^^

:ref:`Common_Value_Attributes`

Value Node elements
------------------------------------------

Overview
~~~~~~~~

A value node is a computed way to get a value, that can depends on other values, value nodes and even current time.

A value node element may be of different types. Current possible nodes are: `'animated'`_, `'static_list'`_, `'dynamic_list'`_, `'bline'`_, `'wplist'`_, `'dilist'`_, `'weighted_average'`_, `'average'`_,  `'canvas'`_, any `Linkable Value Node elements`_, and any `Value elements`_ with or without 'guid' attribute (they are constant, in this case).

.. _value_node_common_atributes:

Common Attributes
^^^^^^^^^^^^^^^^^

:id: <optional> <string> ValueNode identifier, for exported valuenodes. See `'defs'`_.
:guid: <optional> <string> If other value nodes are linked to this one, they refer it by this GUID attribute (if this value node is not exported). Value node Bone must always have its GUID set.

'animated'
------------------------------------------

Overview
~~~~~~~~

A value node that can report a different value at a time. As the name says, pretty useful for animation.

Waypoints sets the values at specific time points, and anything between two waypoints depends on the interpolation set on the consecutive pair of them.

Any type described in `Value elements`_ is supported.

The <animated> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'waypoint'`_ in any order.

Attributes
^^^^^^^^^^

:type: <string> The value type being animated. Any type described in `Value elements`_ is supported.
:interpolation: <optional> How to compute the value at any time between two consecutive waypoints. Supported interpolation are listed in `Common_Value_Attributes`_.

'waypoint'
------------------------------------------

Overview
~~~~~~~~

It is the basic element of an `'animated'`_ value node.

A waypoint sets a value for a layer parameter at a specific time point, and anything between two waypoints depends on the interpolation set on the consecutive pair of them.

The <waypoint> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It must have a single `Value Node elements`_, but only if 'use' attribute is unset.

Attributes
^^^^^^^^^^

:time: <Time> 
:use: <optional> <string> the ID of an exported valuenode in canvas. See `'defs'`_.
:before: <optional> <string> How this waypoint affects the interpolation of the value between this one and the previous waypoint. Supported interpolation are listed in `Common_Value_Attributes`_.
:after: <optional> <string> How this waypoint affects the interpolation of the value between this one and the next waypoint. Supported interpolation are listed in `Common_Value_Attributes`_.
:tension: <option> <real> The value of tension parameter for TCB interpolation.
:temporal-tension: <option> <real> The value of temporal-tension parameter for TCB interpolation.
:continuity: <option> <real> The value of continuity parameter for TCB interpolation.
:bias: <option> <real> The value of bias parameter for TCB interpolation.

'static_list'
------------------------------------------

Overview
~~~~~~~~

A value node that represents a list of values of same type. This list doesn't change its size over time.

Any type described in `Value elements`_ is supported.

The <static_list> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_ in any order.

Attributes
^^^^^^^^^^

:type: <string> The value type of any list entry. Any type described in `Value elements`_ is supported.

'dynamic_list'
------------------------------------------

Overview
~~~~~~~~

A value node that represents a list of values of same type. List entries may be disabled and enabled over time, like it was being removed and (re)inserted.

Any type described in `Value elements`_ is supported.

The <dynamic_list> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_ in any order.

Attributes
^^^^^^^^^^
.. _Common_Dynamic_List_Attributes:
:type: <string> The value type of any list entry. Any type described in `Value elements`_ is supported.
:loop: <optional> <boolean> "true" if it is a looped list (i.e. after last entry it should go back to the first one). Default: "false".

'entry'
------------------------------------------

Overview
~~~~~~~~

It's the basic element of a `'static_list'`_ or `'dynamic_list'`_ value node.

The <entry> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

It must have a single `Value Node elements`_, but only if 'use' attribute is unset.

Attributes
^^^^^^^^^^

:use: <optional> <string> the ID of an exported valuenode in canvas. See `'defs'`_.
:on: <optional> <list of Time Code> A comma-separated list that tells when entry is active. Only for dynamic_list value node types. Time Code is the time point optionally preceded by the priority value (an integer) in format : ``[p{priority} ]{time}``. Brackets ``[]`` mean optional info, and they should be not be in the string. Similar for curly brackets ``{}`` that describe what info whould be there. For time string format, see `'time'` element.
:off: <optional> <list of Time Code> A comma-separated list that tells when entry is inactive. Only for dynamic_list value node types. See above 'on' attribute.

'bline' 
------------------------------------------

Overview
~~~~~~~~

A BLine is a dynamic list where each entry is a Spline vertex. Please refer to `'dynamic_list'`_ , as the only diference is the element name.

The <bline> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_. The order matters.

Attributes
^^^^^^^^^^

:ref:`Common_Dynamic_List_Attributes`

'wplist'
------------------------------------------

Overview
~~~~~~~~

A wplist is a dynamic list where each entry is a Width Point. Please refer to `'dynamic_list'`_ , as the only diference is the element name.

The <wplist> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_ in any order.

Attributes
^^^^^^^^^^

:ref:`Common_Dynamic_List_Attributes`

'dilist'
------------------------------------------

Overview
~~~~~~~~

A dilist is a dynamic list where each entry is a Dash Item. Please refer to `'dynamic_list'`_ , as the only diference is the element name.

The <dilist> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_. The order matters.

Attributes
^^^^^^^^^^

:ref:`Common_Dynamic_List_Attributes`

'weighted_average'
------------------------------------------

Overview
~~~~~~~~

A weighted_average is a dynamic list. Please refer to `'dynamic_list'`_ , as the only diference is the element name.

The <weighted_average> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_ in any order.

Attributes
^^^^^^^^^^

:ref:`Common_Dynamic_List_Attributes`

'average'
------------------------------------------

Overview
~~~~~~~~

A average is a dynamic list. Please refer to `'dynamic_list'`_ , as the only diference is the element name.

The <average> element
~~~~~~~~~~~~~~~~~~~~

Content model
^^^^^^^^^^^^^

Any number of `'entry'`_ in any order.

Attributes
^^^^^^^^^^

:ref:`Common_Dynamic_List_Attributes`

Linkable Value Node elements
------------------------------------------

Overview
~~~~~~~~

Currently, there are 4 fundamental types of Value Nodes: Const, Animated, Placeholder and Linkable Valuenode.

Placeholder is just a temporary value node object used while parsing canvas file on loading.

Const represents a constant… Normally, in saved file, it is used directly a `Value elements`_ instead of explicitly create a Const.

Animated describes a value changing on time via waypoins. See `'animated'`_.

All other value node types are derived from Linkable Value Node, that is a generic value node type whose parameters are indexed and their values are bound to other value nodes! Example of linkable value nodes are `'static_list'`_, `'dynamic_list'`_, `'bline'`_, `'wplist'`_, `'dilist'`_, `'weighted_average'`_, `'average'`_. There are many others, however, but they do not any need special treatment in the SIF file format.

Content model
^^^^^^^^^^^^^

Each element in a linkable value node type element is a parameter. The parameter name is the element name. They are described in `Linkable Value Node Parameter elements`_. It must have at most one instance of each Linkable Value Node Parameter element.

If any parameter is bound to an exported value node (see `'defs'`_), it will be represented by an attribute, instead of an child element.

Common Attributes
^^^^^^^^^^^^^^^^^

:id: <optional> <string> ValueNode identifier, for exported valuenodes. See `'defs'`_.
:type: <string> The value type being handled by the value node. Any type described in `Value elements`_ is supported.
Others: A linkable value node parameter that refers to an exported value node is represent by an atribute. The attribute name is the parameter name, and the attribute value is the value node id.

Linkable Value Node Parameter elements
------------------------------------------

Overview
~~~~~~~~

The name of a Linkable Value Node Parameter element is parameter name itself (without localization).

Content model
^^^^^^^^^^^^^

Same for the value node.

Attributes
^^^^^^^^^^^^^^^^^

Same for a value node element.
