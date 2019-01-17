How to add new Dock Window
==========================

Child windows in Synfig is called docks.
This is because thay are dockable. To add new dock window to Synfig follow this steps:

* Move to synfig-studio/src/gui/docks;
* Add 2 files yourclass_dock.h and yourclass_dock.cpp (_dock in filename is not necessary, but recommended);
* Add your class to /synfig-studio/src/gui/app.h (if you want to call it from other classes)
* Register your dock (best way to do this inside app.cpp)

Step-by-step:

Your window must be inherited from Dockable class 
and be inside studio namespace.
Here the basic code for mydock_dock.h:

.. code-block:: cpp

  #ifndef __SYNFIG_STUDIO_LAYERS_DOCK_H
  #define __SYNFIG_STUDIO_LAYERS_DOCK_H

  #include "docks/dockable.h"
  #include <gtkmm.h>

  namespace studio {

  class MyDock : public Dockable
  {
    public:
        MyDock(const std::string& name,const std::string& local_name, Gtk::StockID stock_id_=Gtk::StockID(" ")): Dockable(name,local_name,stock_id_) {};

  };

  }; // END of namespace studio


  #endif

Adding your class to app.h

.. code-block:: cpp

  class MyDock;

Registering your class (in app.cpp)

.. code-block:: cpp

  #include "docks/mydock_dock.h"

  studio::MyDock* my_dock;

  // add this lines inside App::App constructor

  studio_init_cb.task(_("Init MyDock..."));
  my_dock = new studio::MyDock();
  dock_manager->register_dockable(*my_dock);

Add your files inside Make_insert and CMakeLists.txt (as for now we supporting both build systems).

Now is time to rebuild our makefile (otherwise make will say it can't find your class).

To do this, first you need to know where you building synfig. If you not using non-standard prefix
then you can just:
* move to synfigstudio folder and run:
* ./bootstrap.sh
* ./configure
* make install

If you are using custom build prefix (for example you are building Synfig to your home folder), then:
* look inside config.log file and find ./configure keys.
* run bootstrap.sh
* set PKG_CONFIG_PATH environment variable to /path_where_you_build_synfig/lib/pkgconfig
* run ./configure with keys you got from config.log ealier.

Or otherwise you can just do full build using linux_build script.
