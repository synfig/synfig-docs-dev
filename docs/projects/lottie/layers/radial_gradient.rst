Radial Gradient Layer
=====================

+---------------------+---------------------------------------------------------------------------+
|       Property      |                         Path to Property in lottie                        |
+=====================+===========================================================================+
|       Z depth       |               Depends on ordering of layers in lottie format              |
+---------------------+---------------------------------------------------------------------------+
|        Amount       |   layers/shape.json -> "ef" -> effects/gFill.json -> effects/opacity.json |
+---------------------+---------------------------------------------------------------------------+
|     Blend_method    |Partially supported                                                        |
+---------------------+---------------------------------------------------------------------------+
|        Gradient     |      layers/shape.json -> "ef" -> effects/gfill.json                      |
+---------------------+---------------------------------------------------------------------------+
|       Center        |      layers/shape.json ->  "ef" -> effects/gFill.json ->  "s"             |
+---------------------+---------------------------------------------------------------------------+
|       Radius        |      layers/shape.json ->  "ef" -> effects/gFill.json ->  "e"             |
+---------------------+---------------------------------------------------------------------------+
|      Loop           |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+
|      Zigzag         |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+


Important points
----------------

- Since the lottie format requires both Gradient start point and Gradient end point, Center parameter is used as the gradient start point and for the gradient end point, I have used the radius and added it to the x-axis component of the center paramter to get the x-axis component and used the y-axis component from center parameter for the y-axis component. Therefore, the expression for gradient end point is (start[0] + radius, start[1]).
