.. _copy-paste-exported-values:

Control handling exported values during copy-pasting [PENDING]
===============================================================

In Synfig user can export any parameter of any layer - this way parameter is linked to exported value.
When user copy-pasting such layer from one file into another, the exported values are not copied (they are referenced/linked).
This creates problems

I suggest to show a dialog, which is listing all exported values (ones which pasted layer has).
In this dialog user can select which exported values should be copied and which should be linked from old file (default behavior is to copy values).

What if target file already has exported values with the same name?

I think the dialog should show icons for values marked to copy, which indicate if such exported value already exists in target document. If its type is different, then there should be alarm icon, and dialog should allow to rename exported values.

Here is a prototype:

    .. image:: copy-paste-exported-values/synfig-dialog.png

    .. image:: copy-paste-exported-values/synfig-dialog.kra
    
Explanation:

(1) If checkbox is enabled, to exported value will be copied into targed document.  Default value for checkbox: enabled.

(2) User can uncheck checkboxes, that makes the value will be linked (referenced) from source file.

(3) Clicking this checkbox allows to enable/disable all checkboxes.

(4) This value is copied to target document (see checkbox enabled), but the target document has exported value with the same name and different type. So, that creates conflict. 

(5) This value is copied to target document (see checkbox enabled), and the target document has exported value with the same name and same type. No problem here, icon indicates that this value will be linked to existing exported value in target document.

(6) If value is not copied to target document (checkbox disabled), then a path displayed to file where it is referenced from.

(7) OK button is not active if there is at least one conflict - see (4).

(8) For values which are marked to be copied into target document (checkbox enabled) user can click on their name and edit it (type other text). In any value is renamed then icons - (4) and (5) should updated automatically.
