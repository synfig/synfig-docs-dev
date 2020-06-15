Text Layer
==========

+-----------------+-----------------------------------------------------------------------------------------+
|     Property    |                       Path to Property in lottie                                        |
+=================+=========================================================================================+
|     Z depth     |             Depends on ordering of layers in lottie format                              |
+-----------------+-----------------------------------------------------------------------------------------+
|      Amount     | layers/text.json -> “t” -> “a” -> “o” -> properties/[value.json OR valueKeyframed.json] |
+-----------------+-----------------------------------------------------------------------------------------+
|   Blend_method  |           layers/text.json -> “bm” -> helpers/blendMode.json                            |
+-----------------+-----------------------------------------------------------------------------------------+
|      Text       |           layers/text.json -> “t” -> “d” -> “k” -> “s”-> "t"                            |
+-----------------+-----------------------------------------------------------------------------------------+
|      Color      |           layers/text.json -> “t” -> “a” -> “fc“ ->  effects/color.json                 |
+-----------------+-----------------------------------------------------------------------------------------+
|      Family     |           animation.json -> "fonts" -> "list" -> "fFamily"                              |
+-----------------+-----------------------------------------------------------------------------------------+
|      Style      |           animation.json -> "fonts" -> "list" -> "fStyle"                               |
+-----------------+-----------------------------------------------------------------------------------------+
|      Weight     |           animation.json -> "fonts" -> "list" -> "fWeight"                              |
+-----------------+-----------------------------------------------------------------------------------------+
|      Compress   | layers/text.json -> “t” -> “a” -> “t” -> properties/[value.json OR valueKeyframed.json] |
+-----------------+-----------------------------------------------------------------------------------------+
|      VCompress  |           layers/text.json -> “t” -> “d” -> “k” -> “s”-> "t"                            |
+-----------------+-----------------------------------------------------------------------------------------+
|      Size       |           layers/text.json -> "ks" ->helpers/transform.json -> "s"                      |
+-----------------+-----------------------------------------------------------------------------------------+
|      Orient     |           layers/text.json -> "ks" ->helpers/transform.json -> "a"                      |
+-----------------+-----------------------------------------------------------------------------------------+
|      Origin     |           layers/text.json -> "ks" ->helpers/transform.json -> "p"                      |
+-----------------+-----------------------------------------------------------------------------------------+
|   Use_Kerning   |                                Not Supported                                            |
+-----------------+-----------------------------------------------------------------------------------------+
|     Grid_fit    |                                Not Supported                                            |
+-----------------+-----------------------------------------------------------------------------------------+
|      Invert     |                                Not Supported                                            |
+-----------------+-----------------------------------------------------------------------------------------+


Important points regarding Text Layer
-------------------------------------

- On checking the overlap between fonts supported by Lottie and fonts used by Synfig, the following fonts have been included and are ready to be used : "Sans Serif" ,"Times New Roman", "Calibria", "Arial", "Courier", "Comic Sans". More fonts will be added later as and when Lottie adds support for more fonts.

- Vertical Compression is only available for integer values due to a lack of support for non-integer values.
