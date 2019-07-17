Transform Layer
============

+----------+--------------------------------------------------------------+
| Property |                 Path to Property in lottie                   |
+==========+==============================================================+
|  Origin  | layers/preComp.json -> "ks" -> helpers/transform.json -> "p" |
+----------+--------------------------------------------------------------+


Important points
----------------

- In Lottie, all the parts of shapes/elements lying outside a precomposition are cut off and never displayed. Hence the over all size of a pre-comp is increased by some amount as a precaution. Some part which is outside a pre-comp might come inside the overall canvas after some transformations(rotation/translation/scale). This increase is given by: 
  ``ADDITIONAL_PRECOMP_WIDTH`` and ``ADDITIONAL_PRECOMP_HEIGHT``

- Anchor point is always set to zero. But it is bound to change when the width and height of the pre-composition are changed
