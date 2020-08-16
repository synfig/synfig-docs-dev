Shade Layer
===========

+------------------+--------------------------------------------------------------------+
| Property in .sif |                     Path to property in Lottie                     |
+------------------+--------------------------------------------------------------------+
|      Z_depth     |        Only one shade layer is supported in Lottie per layer       |
+------------------+--------------------------------------------------------------------+
|      Amount      |  “ef” -> Opacity -> properties/[value.json OR valueKeyframed.json] |
+------------------+--------------------------------------------------------------------+
|   Blend Method   |                            Not Supported                           |
+------------------+--------------------------------------------------------------------+
|       Color      |   "ef" -> Shadow Color ->effects/fill.json -> effects/color.json   |
+------------------+--------------------------------------------------------------------+
|      Origin      | “ef” -> Distance -> properties/[value.json OR valueKeyframed.json] |
+------------------+--------------------------------------------------------------------+
|       Size       | “ef” -> Softness -> properties/[value.json OR valueKeyframed.json] |
+------------------+--------------------------------------------------------------------+
|       Type       |              Only Gaussian Blur is supported in Lottie             |
+------------------+--------------------------------------------------------------------+
|      Invert      |                       Not Supported in Lottie                      |
+------------------+--------------------------------------------------------------------+

Important points
----------------

- For all types of shade layer, they are defaulted to Gaussian Blur, since this is the only blur type supported by Lottie. 
- Shade in lottie is not supported for outlines.