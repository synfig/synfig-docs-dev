Blur Layer
==========

+------------------+---------------------------------------------------------------+
| Property in .sif |                   Path to Property in lottie                  |
+==================+===============================================================+
|      Z_depth     |         Depends on ordering of layers in lottie format        |
+------------------+---------------------------------------------------------------+
|      Amount      |                         Not Supported                         |
+------------------+---------------------------------------------------------------+
|   Blend Method   |                         Not Supported                         |
+------------------+---------------------------------------------------------------+
|       Size       | “ef” -> "v" -> properties/[value.json OR valueKeyframed.json] |
+------------------+---------------------------------------------------------------+
|       Type       |            All types are defaulted to Gaussian Blur           |
+------------------+---------------------------------------------------------------+

Important points
----------------

- For all types of blur layer, they are defaulted to Gaussian Blur, since this is the only blur type supported by Lottie. 
- Rendering time increases as you increase the number of images to blur.
- Amount and Blend Method is not supported for blur in Lottie.