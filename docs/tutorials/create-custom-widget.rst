========================
Creating a custom widget
========================

For creating its graphical user interface, Synfig Studio uses `gtkmm <https://www.gtkmm.org>`_, the official C++ binding for the `Gtk <https://www.gtk.org>`_.
This toolkit provides `a lot of widgets <https://developer.gnome.org/gtk3/stable/ch03.html>`_, but sometimes they don't exactly fulfill our needs.
In those situations, we have/opt to create our custom widgets, and that's what this page is about.

For creating custom gtkmm widgets, first be sure you properly understand gtkmm. You can study it by reading `its official documentation <https://developer.gnome.org/gtkmm-tutorial/stable/chapter-basics.html.en>`_.
They do explain `how to create custom widgets <https://developer.gnome.org/gtkmm-tutorial/stable/chapter-customwidgets.html.en>`_ and `custom signals <https://developer.gnome.org/gtkmm-tutorial/stable/chapter-custom-signals.html.en>`_.
Of course, they have `their API reference <https://developer.gnome.org/gtkmm/stable/annotated.html>`_, split in the Gtk modules.
Sometimes, however, it may be somehow… lacking. You can always check the `Gtk docs <https://developer.gnome.org/gtk3/stable/>`_, instead of gtkmm's, to check some confusing info.
Both API reference are also available on Linux via `devhelp <https://wiki.gnome.org/Apps/Devhelp>`_ app.

---------
Checklist
---------

* the class should be named Widget_* Examples: Widget_Link, Widget_Timetrack.
* its files should be placed in synfig-studio/gui/widgets folder
* if a file contains translatable text, it should be mentioned in synfig-studio/po/POTFILES.in
* signal slots/callbacks should be named on_[signal_name] or on_[child_widget_name]_[signal_name]
* the user-interaction callbacks, like on_key_pressed and on_draw, if used, should catch all exceptions, if there is any of synfig:: or synfigapp:: API call in the method implementation, even indirectly. It should be done by `macros SYNFIG_EXCEPTION_GUARD_* <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/exception_guard.h>`_
* it should `support Gtk::Builder and be available for Glade <https://github.com/synfig/synfig/pull/900/commits/025eec22c849c45d3c9e1fa295459033702ed069>`_
 * it should have properties defined via Glib::Property (for better support for Glade editor)
* if it's a widget that uses time as horizontal axis, like for Widget_SoundWave, Widget_Timetrack and Widget_Curves, it could use `Widget_TimeGraphBase <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/widgets/widget_timegraphbase.h>`_ as a parent class, for useful methods.
* please document it :P
