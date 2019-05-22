.. _supported:

Supported Features
==================

Star Layer
----------
+-----------------+---------------------+---------------------------------------------+
|  Property Name  |        Value        | Type(Animation)                             |
+-----------------+---------------------+---------------------------------------------+
|     Z depth     |      Supported      | Animation not supported                     |
+-----------------+---------------------+---------------------------------------------+
|      Amount     |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|   Blend_method  | Partially supported | Not needed                                  |
+-----------------+---------------------+---------------------------------------------+
|      Color      |      Supported      | TCB and Clamped interpolation not supported |
+-----------------+---------------------+---------------------------------------------+
|      Origin     |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|      Invert     |    Not Supported    | Not Supported                               |
+-----------------+---------------------+---------------------------------------------+
|   Antialiasing  |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|     Feather     |    Not supported    | Not Supported                               |
+-----------------+---------------------+---------------------------------------------+
| Type of Feather |    Not supported    | Not Supported                               |
+-----------------+---------------------+---------------------------------------------+
|  Winding Style  |    Not Supported    | Not Supported                               |
+-----------------+---------------------+---------------------------------------------+
|   Outer Radius  |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|   Inner Radius  |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|      Angle      |      Supported      | Supported                                   |
+-----------------+---------------------+---------------------------------------------+
|      Points     |      Supported      | [Waiting for bug fix]                       |
+-----------------+---------------------+---------------------------------------------+
| Regular Polygon |      Supported      | [Devising the algorithm]                    |
+-----------------+---------------------+---------------------------------------------+

Circle Layer
------------
+---------------+---------------------+---------------------------------------------+
| Property Name |        Value        |               Type(Animation)               |
+---------------+---------------------+---------------------------------------------+
|    Z depth    |      Supported      |           Animation not supported           |
+---------------+---------------------+---------------------------------------------+
|     Amount    |      Supported      |                  Supported                  |
+---------------+---------------------+---------------------------------------------+
|  Blend_method | Partially supported |                  Not needed                 |
+---------------+---------------------+---------------------------------------------+
|     Color     |      Supported      | TCB and Clamped interpolation not supported |
+---------------+---------------------+---------------------------------------------+
|     Radius    |      Supported      |                  Supported                  |
+---------------+---------------------+---------------------------------------------+
|     Origin    |      Supported      |                  Supported                  |
+---------------+---------------------+---------------------------------------------+
|     Invert    |    Not Supported    |                Not Supported                |
+---------------+---------------------+---------------------------------------------+
|    Feather    |    Not supported    |                Not Supported                |
+---------------+---------------------+---------------------------------------------+

Blend Methods
-------------
- The blend methods supported are: Composite, Difference, Multiply, Hard light, Luminance, Saturation, Hue, Color, Darken, Brighten, Overlay, Screen.
- These methods have somewhat different implementations in Lottie and Synfig, hence the blend methods tend to show different behaviour when a layer is blended over a transparent background.
