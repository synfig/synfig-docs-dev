Star Layer
==========

+-----------------+------------------------------------------------------------------------+
|     Property    |                       Path to Property in lottie                       |
+=================+========================================================================+
|     Z depth     |             Depends on ordering of layers in lottie format             |
+-----------------+------------------------------------------------------------------------+
|      Amount     | layers/solid.json -> "ef" -> effects/fill.json -> effects/opacity.json |
+-----------------+------------------------------------------------------------------------+
|   Blend_method  |           layers/solid.json -> “bm” -> helpers/blendMode.json          |
+-----------------+------------------------------------------------------------------------+
|      Color      |  layers/solid.json -> "ef" -> effects/fill.json -> effects/color.json  |
+-----------------+------------------------------------------------------------------------+
|      Origin     |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+
|      Invert     |             layers/solid.json -> helpers/mask.json -> "inv"            |
+-----------------+------------------------------------------------------------------------+
|   Antialiasing  |             This property is always present(true) in lottie            |
+-----------------+------------------------------------------------------------------------+
|     Feather     |                              Not Supported                             |
+-----------------+------------------------------------------------------------------------+
| Type of Feather |                              Not supported                             |
+-----------------+------------------------------------------------------------------------+
|  Winding Style  |                              Not supported                             |
+-----------------+------------------------------------------------------------------------+
|   Outer Radius  |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+
|   Inner Radius  |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+
|      Angle      |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+
|      Points     |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+
| Regular Polygon |     Calculation of  layers/solid.json -> helpers/mask.json -> "pt"     |
+-----------------+------------------------------------------------------------------------+

Important points regarding conversion
-------------------------------------

- Ordering of Z_depth is .sif format is opposite from lottie format. The first layer in the .sif format will be the farthest or deepest from the observer whereas the first layer in lottie format will be the   closest to the observer.

- Instead of directly using the shapes layer from Lottie, a solid region layer is used and is masked(using masks) to draw a star/polygon. This is done to support the invert parameter from Synfig.

- The mask is drawn according to: https://github.com/synfig/synfig/blob/678cc3a7b1208fcca18c8b54a29a20576c499927/synfig-core/src/modules/mod_geometry/star.cpp
