Radial Gradient Layer
=====================

+---------------------+---------------------------------------------------------------------------+
|       Property      |                         Path to Property in lottie                        |
+=====================+===========================================================================+
|       Z depth       |               Depends on ordering of layers in lottie format              |
+---------------------+---------------------------------------------------------------------------+
|        Amount       |   layers/shape.json -> "ef" -> shapes/gFill.json -> effects/opacity.json  |
+---------------------+---------------------------------------------------------------------------+
|     Blend_method    |                            Partially supported                            |
+---------------------+---------------------------------------------------------------------------+
|       Gradient      |                 layers/shape.json -> "ef" -> shapes/gfill.json            |
+---------------------+---------------------------------------------------------------------------+
|       Center        |              layers/shape.json ->  "ef" -> shapes/gFill.json ->  "s"      |
+---------------------+---------------------------------------------------------------------------+
|       Radius        |              layers/shape.json ->  "ef" -> shapes/gFill.json ->  "e"      |
+---------------------+---------------------------------------------------------------------------+
|       Loop          |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+
|       Zigzag        |                               Not supported                               |
+---------------------+---------------------------------------------------------------------------+


Important points
----------------

- Since the lottie format requires both Gradient start point and Gradient end point, Center parameter is used as the gradient start point and for the gradient end point, I have used the radius and added it to the x-axis component of the center paramter to get the x-axis component and used the y-axis component from center parameter for the y-axis component. Therefore, the expression for gradient end point is (start[0] + radius, start[1]).
- Since, only gradient fill is supported in Lottie and not gradient ramp, to create the Radial Gradient layer, I introduced two parameters point1 and point2 which are used to create a rectangle layer first which fills the whole canvas in which gradient fill is used.
