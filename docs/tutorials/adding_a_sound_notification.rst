===========================
Adding a Sound notification
===========================

After the implementation of `#648 - GUI: Play a sound when rendering is done <https://github.com/synfig/synfig/issues/648#>`_,

it could be useful to keep a track of the way to implement a new sound notification in SynfigStudio

This tutorial will be divided in 2 parts:

* Adding the sound notification
* Adding an option to Preferences dialog

----

-----------------------------
Adding the sound notification
-----------------------------

~~~~~~~~~~~~~~~~~~~~~
synfig-studio/sounds/
~~~~~~~~~~~~~~~~~~~~~
* Add your sample in this directory in your source tree.
* Add a notice about the license of your sample in *readme.txt* (*the asset should have a permissive license*)
* Add your sound sample to the list in *Makefile.am*

~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/app.h
~~~~~~~~~~~~~~~~~~~~~~~~~~~
Declare the sound (Mix_Chunk*) as a static member.

.. code-block:: cpp

 //The sound effects that will be used
 static Mix_Chunk* gRenderDone;

*You could add a bool declared as a static member for an option to play the sound (to be defined in Preferences dialog).*

.. code-block:: cpp

 static bool use_render_done_sound;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/app.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Initialize the sound (Mix_Chunk*) to NULL.

You could find the gRenderDone and add it after.

.. code-block:: cpp

 Mix_Chunk* App::gRenderDone = NULL;


*As well you may initialize the bool for allow to play the sound; this will be connected later in the Preferences dialog*

.. code-block:: cpp

 bool App::use_render_done_sound = true;

* Locate this part (at the end of the constructor App::App )

::

 path_to_sounds += ETL_DIRECTORY_SEPARATOR;

and load your sound as follow (the volume is set at 50% if success)

.. code-block:: cpp

 //Load sound effects
 App::gRenderDone = Mix_LoadWAV( (path_to_sounds + "renderdone.wav").c_str() );
 if ( App::gRenderDone == NULL ) {
     synfig::error( _("SDL_mixer could not load gRenderDone : %s\n"), Mix_GetError() );
 }  else {
     Mix_VolumeChunk(App::gRenderDone, MIX_MAX_VOLUME/2);
 }

* Finally, free the sound in the destructor App::~App (and NULL the pointer)

.. code-block:: cpp

 //<!- ----- SDL2 - Sound effects -----
 //Free the sound effects
 Mix_FreeChunk( App::gRenderDone );
 App::gRenderDone = NULL;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/<yourSource>.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You should add

.. code-block:: cpp

 #include <SDL2/SDL.h>
 #include <SDL2/SDL_mixer.h>

Add your sound effect play where it should be (end of an event for example)

You have 2 possibilities after this

1) Play directly without control in the preferences

.. code-block:: cpp

 //Sound effect - RenderDone (-1 : play on first free channel, 0 : no repeat)
 Mix_PlayChannel( -1, App::gRenderDone, 0 );
 
**or**

2) Play according the preferences

.. code-block:: cpp

 //Sound effect - RenderDone (-1 : play on first free channel, 0 : no repeat)
 if (App::use_render_done_sound) Mix_PlayChannel( -1, App::gRenderDone, 0 );

----

--------------------------------------
Adding an option to Preferences dialog
--------------------------------------

In the first part we added use_render_done_sound to prepare a preference option.

It will be a toggle *toggle_play_sound_on_render_done* in Preferences/Render tab

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/dialogs/dialog_setup.h
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* In class Dialog_Setup

add the declaration of the "changed" handler

.. code-block:: cpp

 void on_play_sound_on_render_done_changed();

and the corresponding Switch control

.. code-block:: cpp

 Gtk::Switch toggle_play_sound_on_render_done;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/dialogs/dialog_setup.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Find in which page and which position you will add your option (named *toggle_play_sound_on_render_done*, here in *Dialog_Setup::create_render_page(PageInfo pi)*)

.. code-block:: cpp

 // Render - Render Done sound
 attach_label(pi.grid, _("Chime on render done"), ++row);
 pi.grid->attach(toggle_play_sound_on_render_done, 1, row, 1, 1);
 toggle_play_sound_on_render_done.set_halign(Gtk::ALIGN_START);
 toggle_play_sound_on_render_done.set_hexpand(false);
 toggle_play_sound_on_render_done.set_tooltip_text(_("A chime is played when render has finished."));
 toggle_play_sound_on_render_done.property_active()
                                 .signal_changed()
                                 .connect(sigc::mem_fun(*this, &Dialog_Setup::on_play_sound_on_render_done_changed));

* In *Dialog_Setup::on_apply_pressed()*

.. code-block:: cpp

 // Set the use of a render done sound
 App::use_render_done_sound = toggle_play_sound_on_render_done.get_active();

* Create the handler

.. code-block:: cpp

 void
 Dialog_Setup::on_play_sound_on_render_done_changed()
 {
     App::use_render_done_sound = toggle_play_sound_on_render_done.get_active();
 }

* In *Dialog_Setup::refresh()*

.. code-block:: cpp

 // Refresh the status of the render done sound flag
 toggle_play_sound_on_render_done.set_active(App::use_render_done_sound);

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/app.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* In *class Preferences*,

among the saving parts

.. code-block:: cpp

 if(key=="use_render_done_sound")
 {
     value=strprintf("%i",(int)App::use_render_done_sound);
     return true;
 }

and among the loading parts

.. code-block:: cpp

 if(key=="use_render_done_sound")
 {
     int i(atoi(value.c_str()));
     App::use_render_done_sound=i;
     return true;
 }

* In the ret.push_back part, add

.. code-block:: cpp

 ret.push_back("use_render_done_sound");

* In *restore_default_settings()*,

add a default value

.. code-block:: cpp

 synfigapp::Main::settings().set_value("use_render_done_sound", "1");

----

Don't forget that some parts have been done in the first section

Hoping it will be sufficient to understand the whole system :)