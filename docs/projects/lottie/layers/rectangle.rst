Rectangle Layer
===============

+---------------------+---------------------------------------------------------------------------+
|       Property      |                         Path to Property in lottie                        |
+=====================+===========================================================================+
|       Z depth       |               Depends on ordering of layers in lottie format              |
+---------------------+---------------------------------------------------------------------------+
|        Amount       |   layers/solid.json -> "ef" -> effects/fill.json -> effects/opacity.json  |
+---------------------+---------------------------------------------------------------------------+
|     Blend_method    |            layers/solid.json -> “bm” -> helpers/blendMode.json            |
+---------------------+---------------------------------------------------------------------------+
|        Color        |    layers/solid.json -> "ef" -> effects/fill.json -> effects/color.json   |
+---------------------+---------------------------------------------------------------------------+
|       Point 1       |      Calculation of  layers/solid.json -> helpers/mask.json -> "pt"       |
+---------------------+---------------------------------------------------------------------------+
|       Point 2       |      Calculation of  layers/solid.json -> helpers/mask.json -> "pt"       |
+---------------------+---------------------------------------------------------------------------+
|        Expand       |      Calculation of  layers/solid.json -> helpers/mask.json -> "pt"       |
+---------------------+---------------------------------------------------------------------------+
|        Invert       |             layers/solid.json -> helpers/mask.json -> "inv"               |
+---------------------+---------------------------------------------------------------------------+
|      Feather X      |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+
|      Feather Y      |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+
|        Bevel        |      Calculation of  layers/solid.json -> helpers/mask.json -> "pt"       |
+---------------------+---------------------------------------------------------------------------+
| Keep Bevel Circular |      Calculation of  layers/solid.json -> helpers/mask.json -> "pt"       |
+---------------------+---------------------------------------------------------------------------+

Important points
----------------

- Instead of directly using the shapes layer from Lottie, a solid region layer is used and is masked(using masks) to draw a rectangle. This is done to support the invert parameter from Synfig.

- The mask is drawn according to: https://github.com/synfig/synfig/blob/678cc3a7b1208fcca18c8b54a29a20576c499927/synfig-core/src/modules/mod_geometry/rectangle.cpp
