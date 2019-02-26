============================
Adding a Render progress bar
============================

Now let's implement a Render Progress Bar!

It has been requested several time:

* `Indication needed that rendering is in progress. #383 <https://github.com/synfig/synfig/issues/383>`_
* `Feature request: Give feedback when rendering is happening and complete #626 <https://github.com/synfig/synfig/issues/383>`_
* also in `Default render parameter are bad #464 <https://github.com/synfig/synfig/issues/464>`_

Summary

* 1 Things to take into account

 * 1.1 `Where to place the Progress Bar?`_
 * 1.2 `How does really a render work?`_
 * 1.3 `Where to do calls and implementation?`_

* 2 Implementation

 * 2.1 `synfig-studio/src/gui/docks/dock_info.h`_
 * 2.2 `synfig-studio/src/gui/docks/dock_info.cpp`_
 * 2.3 `synfig-studio/src/gui/docks/app.h`_
 * 2.4 `synfig-studio/src/gui/docks/app.cpp`_
 * 2.5 `synfig-studio/src/gui/docks/render.cpp`_
 * 2.6 `synfig-studio/src/gui/docks/asyncrender.cpp`_

---------------------------
Things to take into account
---------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Where to place the Progress Bar?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I chose to implement it in the Dock_Info panel.

After all, this is a kind of information!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
How does really a render work?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**App** (`.h <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/app.h>`__ `.cpp <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/app.cpp>`__) is the root of everything and used to store globals.

But we start in fact from **CanvasView** (`.h <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/canvasview.h>`__ `.cpp <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/canvasview.cpp>`__) which contains the call to display the **RenderSettings** Dialog (`.h <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/asyncrenderer.h>`__ `.cpp <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/asyncrenderer.cpp>`__).

Then we press on render button which leads to execute an **AsyncRenderer** (`.h <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/asyncrenderer.h>`__ `.cpp <https://github.com/synfig/synfig/blob/master/synfig-studio/src/gui/asyncrenderer.cpp>`__) (or 2, sequentially, if we have a second pass for Alpha extraction).

**AsyncRenderer** can have 4 types of targets:

* AsyncTarget_Cairo
* AsyncTarget_Cairo_Tile
* AsyncTarget_Scanline
* AsyncTarget_Tile

Only *AsyncTarget_Cairo* and *AsyncTarget_Scanline* have a *frame_ready()* function that we will use to implement our call to update to the Render ProgressBar.

Note that we can have 2 passes, this has to be considered when displaying the percents of accomplished render.

The render itself is done writing the data to the target, then the selected codec is fed through a pipe (for example ffmpeg).

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Where to do calls and implementation?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In different files, as it is a "multi-level" task.

The details are described in each file touched in the next section.

----

--------------
Implementation
--------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/dock_info.h
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Declare the components and members

* In #include section:

.. code-block:: cpp

 #include <gtkmm/progressbar.h>
 In private section:

 Gtk::ProgressBar render_progress;

 //! Number of passes request - 1 or 2 (if alpha)
 int              n_passes_requested;
 //! Number of passes pending - 2,1,0
 int              n_passes_pending;

* In public section:

.. code-block:: cpp

 //! Current render progress - 0.0 to 1.0
 //  depends on n_passes_requested and current_pass
 void set_render_progress   (float value);
 void set_n_passes_requested(int   value);
 void set_n_passes_pending  (int   value);

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/dock_info.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Here we will implement the UI and members

* In #include section:

.. code-block:: cpp

 #include "app.h"
 #include <gtkmm/progressbar.h>

*It will permit to access our App::dock_info\_ as a static from anywhere in the application*

* In Dock_Info(), at the end: just before table->show_all();

.. code-block:: cpp

 //Render Progress Bar
 table->attach(*manage(new Gtk::Label(_("Render Progress: "))),0,1,5,6,Gtk::EXPAND|Gtk::FILL,Gtk::SHRINK|Gtk::FILL);
 table->attach(render_progress,                                0,5,6,7,Gtk::EXPAND|Gtk::FILL,Gtk::SHRINK|Gtk::FILL);
 render_progress.set_show_text(true);
 render_progress.set_text(strprintf("%.1f%%", 0.0));
 render_progress.set_fraction(0.0);
 //Another spacer
 table->attach(*manage(new Gtk::Label),0,5,7,8);
 
*and after add(\*table);*

.. code-block:: cpp

 //Render progress
 set_n_passes_requested(1); //Default
 set_n_passes_pending  (0); //Default
 set_render_progress (0.0); //Default, 0.0%

* Then at the end of the file, we add these 3 functions:

.. code-block:: cpp

 void studio::Dock_Info::set_n_passes_requested(int value)
 {
     n_passes_requested = value;
 }
 	
 void studio::Dock_Info::set_n_passes_pending(int value)
 {
    n_passes_pending = value;
 }
 
 void studio::Dock_Info::set_render_progress(float value)
 {
   float coeff        = (1.000 / (float)n_passes_requested);  //% of fraction for 1 pass if more than 1 pass
   float already_done = coeff * (float)(n_passes_requested - n_passes_pending -1); 
   float r = ( coeff * value ) + already_done;

   render_progress.set_text( strprintf( "%.1f%%", r*100 ));
   render_progress.set_fraction(r);
 }

The 2 first ones are obvious, the last one does the calculation for the display of the current percents of the WHOLE TASK.

If we have only 1 pass, value will be reflected directly.

In case of 2 (or more, who knows what will be implemented later!), each pass will still continue to send its progress as if it was the only one in the world; we will do the adjustments here.

* 100% of pass 1 while be displayed as 50%.
* 100% of pass 2 while be displayed as 100%.
* If we had 3 passes, it would be 33.3%, 66.6% and 100.0%

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/app.h
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Lets add our *static dock_info\_*. A *studio::dock_info* is already defined in the .cpp but we need to access it as *App::dock_info\_*!

* Inside the static declarations:

.. code-block:: cpp

 static Dock_Info* dock_info_; //For Render ProgressBar

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/app.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Let's declare the static

* Inside the declare of statics

.. code-block:: cpp

 Dock_Info* App::dock_info_            = 0;

* At the end of the constructor App()

.. code-block:: cpp

 App::dock_info_ = dock_info;

It looks like some kind of alias!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/render.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Here the things start to become serious

* In the #include section:

.. code-block:: cpp
 
 #include "docks/dockmanager.h"
 #include "docks/dock_info.h"


* In *RenderSettings::on_render_pressed*

*Just before submit_next_render_pass();*

.. code-block:: cpp

 App::dock_info_->set_n_passes_requested(render_passes.size());
 App::dock_info_->set_n_passes_pending(render_passes.size());
 App::dock_info_->set_render_progress(0.0);
 App::dock_manager->find_dockable("info").present(); //Bring Dock_Info to front
 
*We initialized our ProgressBar with its default parameters to display 0.0%*

*Note that the Dock_Info will be brought to front to show the progression... It's its goal!*


* In RenderSettings::submit_next_render_pass()

*Just after render_passes.pop_back();*

.. code-block:: cpp

 App::dock_info_->set_n_passes_pending(render_passes.size()); //! Decrease until 0
 App::dock_info_->set_render_progress(0.0); //For this pass

*We reinitialized the parameters for this specific pass!*

**Doing tests I noticed that with extract alpha option on, we have 2 passes and therefore the render done sound was played 2 times!**

Let's correct this bad behaviour In RenderSettings::on_finished(), around submit_next_render_pass();

.. code-block:: cpp

 bool really_finished = (render_passes.size() == 0); //Must be checked BEFORE submit_next_render_pass();
 submit_next_render_pass();

 //Sound effect - RenderDone (-1 : play on first free channel, 0 : no repeat)
 if (App::use_render_done_sound) Mix_PlayChannel( -1, App::gRenderDone, 0 );
 if (really_finished) { //Because of multi-pass render
     if (App::use_render_done_sound) Mix_PlayChannel( -1, App::gRenderDone, 0 );
     App::dock_info_->set_render_progress(1.0);
 }

*This way, it will play only once the full render has occured!*

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
synfig-studio/src/gui/docks/asyncrender.cpp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 Now the deepest part

* In #include section:

.. code-block:: cpp

 #include <docks/dock_info.h>
 In the beginning of *AsyncRenderer::start()*

 App::dock_info_->set_render_progress(0.0);

* In *void frame_ready()* **of both** *AsyncTarget_Cairo* and *AsyncTarget_Scanline*

*After ready_next=true;*

.. code-block:: cpp

 int n_total_frames_to_render = warm_target->desc.get_frame_end()        //120
                              - warm_target->desc.get_frame_start()      //0
                              + 1;                                       //->121
 int current_rendered_frames_count = warm_target->curr_frame_
                                   - warm_target->desc.get_frame_start();
 float r = (float) current_rendered_frames_count 
         / (float) n_total_frames_to_render;
 App::dock_info_->set_render_progress(r);

Here the current progress is calculated according the starting, ending and current frame, in a range of 0.0 to 1.0

The pass (or target) thinks it is the only one in the world but it is compensated in the display :)

----

Hoping this will help you to come and join the effort in development of Synfig :)