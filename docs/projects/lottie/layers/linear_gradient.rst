Linear Gradient Layer
=====================

+---------------------+---------------------------------------------------------------------------+
|       Property      |                         Path to Property in lottie                        |
+=====================+===========================================================================+
|       Z depth       |               Depends on ordering of layers in lottie format              |
+---------------------+---------------------------------------------------------------------------+
|       Amount        |   layers/shape.json -> "ef" -> shapes/gFill.json -> effects/opacity.json  |
+---------------------+---------------------------------------------------------------------------+
|    Blend_method     |                            Partially supported                            |
+---------------------+---------------------------------------------------------------------------+
|      Gradient       |               layers/shape.json -> "ef" -> shapes/gfill.json              |
+---------------------+---------------------------------------------------------------------------+
|        p1           |             layers/shape.json ->  "ef" -> shapes/gFill.json ->  "s"       |
+---------------------+---------------------------------------------------------------------------+
|        p2           |             layers/shape.json ->  "ef" -> shapes/gFill.json ->  "e"       |
+---------------------+---------------------------------------------------------------------------+
|       Loop          |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+
|      Zigzag         |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+

Important points
----------------

- p1 and p2 parameters are used to calculate the gradient start point -> "s" and the gradient end point -> "e" respectively.
- Since, only gradient fill is supported in Lottie and not gradient ramp, to create the Linear Gradient layer, I introduced two parameters point1 and point2 which are used to create a rectangle layer first which fills the whole canvas in which gradient fill is used.