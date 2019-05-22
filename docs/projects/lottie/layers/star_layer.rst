Star Layer
==========

Table of Mappings
-------------------

+------------------+------------------------------------------------------------------------------------------------------------------------------+
| Property in .sif |                                                  Path to Property in lottie                                                  |
+==================+==============================================================================================================================+
|      Z_depth     |                                        Depends on ordering of layers in lottie format                                        |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|      Amount      |                         layers/fill.json -> “o” -> properties/[value.json OR valueKeyframed.json]                            |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|   Blend_method   |                                      layers/shape.json -> “bm” -> helpers/blendMode.json                                     |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|       Color      |                                                layers/fill.json -> “ty” = “fl”                                               |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|      Origin      | layers/shape.json -> “it” -> shapes/star.json -> “p” -> properties/[multiDimensional.json OR multiDimensionalKeyframed.json] |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|      Invert      |                              layers/shape.json -> “maskProperties” ->helpers/mask.json -> “inv”                              |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|   Antialiasing   |                                        This property is always present(true) in lottie                                       |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|      Feather     |                                                         Not supported                                                        |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|  Type of Feather |                                                         Not supported                                                        |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|   Winding Style  |                                                         Not Supported                                                        |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|   Outer Radius   |                                     layers/shape.json -> “it” -> shapes/star.json -> “or”                                    |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|   Inner Radius   |            layers/shape.json -> “it” -> shapes/star.json -> “ir” -> properties/[value.json OR valueKeyframed.json]           |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|       Angle      |            layers/shape.json -> “it” -> shapes/star.json -> “r” -> properties/[value.json OR valueKeyframed.json]            |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|      Points      |            layers/shape.json -> “it” -> shapes/star.json -> “pt” -> properties/[value.json OR valueKeyframed.json]           |
+------------------+------------------------------------------------------------------------------------------------------------------------------+
|  Regular Polygon |                               layers/shape.json -> “it” -> shapes/star.json -> “sy” -> “oneOf”                               |
+------------------+------------------------------------------------------------------------------------------------------------------------------+

.. note::
    "it" is used for shapes within groups. When only a single shape is there, "shapes" will be used

Important points regarding conversion
-------------------------------------

- Ordering of Z_depth is .sif format is opposite from lottie format. The first layer in the .sif format will be the farthest or deepest from the observer whereas the first layer in lottie format will be the   closest to the observer.

- ``Amount(lottie) = Amount(.sif) * 100``

- For origin unit conversion, refer to :ref:`miscellaneous <miscellaneous>` section.

- Inner Radius and Outer Radius in .sif format are stored in units. To convert them to pixels(used in lottie), 

  ``ir(lottie) = 1 unit(pixels) * Inner Radius(.sif)``

- For angle conversion, refer to :ref:`miscellaneous <miscellaneous>` section to know the orientation difference in x-y plane in both the formats.
  
  ``angle_lottie = (90 - angle_sif) % 360``
