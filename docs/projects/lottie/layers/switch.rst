Switch Group Layer
==================

+-------------------+-----------------------------------------------------------------------+
|      Property     |                       Path to Property in lottie                      |
+===================+=======================================================================+
|      Z depth      |             Depends on ordering of layers in lottie format            |
+-------------------+-----------------------------------------------------------------------+
|       Amount      |      layers/preComp.json -> "ks" -> helpers/transform.json -> "o"     |
+-------------------+-----------------------------------------------------------------------+
|    Blend_method   |         layers/preComp.json -> “bm” -> helpers/blendMode.json         |
+-------------------+-----------------------------------------------------------------------+
|       Origin      |    layers/preComp.json -> "ks" -> helpers/transform.json -> "p"/"a"   |
+-------------------+-----------------------------------------------------------------------+
|   Transformation  |         layers/preComp.json -> "ks" -> helpers/transform.json         |
+-------------------+-----------------------------------------------------------------------+
|       Canvas      |                          sources/preComp.json                         |
+-------------------+-----------------------------------------------------------------------+
|       Speed       |              layers/preComp.json -> "tm"(time remapping)              |
+-------------------+-----------------------------------------------------------------------+
|    Time Offset    |               layer/preComp.json -> "tm"(time remapping)              |
+-------------------+-----------------------------------------------------------------------+
|   Lock selection  |                               Not needed                              |
+-------------------+-----------------------------------------------------------------------+
|    Outline Grow   |               Global parameter -> settings.OUTLINE_GROW               |
+-------------------+-----------------------------------------------------------------------+
| Active Layer Name | Opacity of layers in canvas is modified according to the active layer |
+-------------------+-----------------------------------------------------------------------+

Important points
----------------

- In Lottie, all the parts of shapes/elements lying outside a precomposition are cut off and never displayed. Hence the over all size of a pre-comp is increased by some amount as a precaution. Some part which is outside a pre-comp might come inside the overall canvas after some transformations(rotation/translation/scale). This increase is given by:
  ``ADDITIONAL_PRECOMP_WIDTH`` and ``ADDITIONAL_PRECOMP_HEIGHT``
