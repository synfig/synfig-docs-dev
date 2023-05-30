.. _renderer_target_surface:

Target and Surface
==================

Synfig supports rendering to different file formats and uses different modules for writing to those file formats. These modules are called Targets. They inherit from the ``Target`` class and can be selected by the user or automatically.

Selecting Target
~~~~~~~~~~~~~~~~

Synfig stores a dictionary(key-value pair) of all available targets with their name as the key and a factory function as value(``book``). It keeps another dictionary where the file extension to which this target can write is used as the key and the target's name as value(``ext_book``).

Macros are used to fill these dictionaries; check ``synfig-core/src/modules/mod_imagemagick/main.cpp`` and ``synfig-core/src/synfig/module.h``.

Rendering to a Target
~~~~~~~~~~~~~~~~~~~~~

Synfig calls the ``Target::render(...)`` function to start the rendering process. The function is responsible for rendering each frame and then writing the output files. Progress is reported using ``ProgressCallback`` passed as the function parameter.

Target_Scanline
---------------

``Target_Scanline`` is the base class for most targets. It writes the output of Tasks to files line by line. The frame-by-frame render loop looks like this:

.. code-block:: cpp

    do
    {
        frame = next_frame(t); // increase frame and time, returns 0 when no frames left

        start_frame();

        // create a new surface
        surface = new SurfaceResource();

        // build and execute tasks
        call_renderer();
        
        for(int y = 0; y < height; y++)
        {
            Color* colorData = start_scanline(y);
            // fill values from surface to colorData
            end_scanline(); // finally write scanline(colorData) to file
        }

        end_frame();
    } while(frame);

The functions ``start_scanline`` and ``end_scaline`` are overridden by modules. The actual data is written to file in these functions only.

Surface
~~~~~~~

See file ``synfig-core/src/synfig/surface.cpp``.

Tasks exchange pixels using Surfaces. Tasks do not write to Targets directly. They write on Surfaces given to them by the Targets. Surfaces store actual pixel data. For OpenGL, a surface is like a Framebuffer.

The ``Surface`` base class only declares essential virtual functions like ``create_vfunc`` for creating a new Surface of this type, ``assign_vfunc`` for assigning data from another surface to this surface, etc.

Since the Cobra engine is multi-threaded and supports different render engines(ex. software and hardware), there are a few requirements that Surfaces must meet:

* Reading and writing from multiple threads with proper locking mechanisms must be possible.
* There should be an easy way to convert Surfaces from one type to another.

Thread-Safety
-------------

Synfig ensures thread-safety of Surfaces using ``std::mutex`` and ``Glib::Threads::RWLock``. To keep locking Surfaces simple, these are not used directly but by ``SurfaceResource::LockBase``. To safely read from a Surface, all you need to do is:

.. code-block:: cpp

   SurfaceResource::LockRead<SurfaceSW> lock(resource); // read locks the surface, unlocks on going out of scope(desctructor called)

   const Surface surface = lock->get_surface(); // calls get_surface() of SurfaceSW

Conversion
----------

``SurfaceResource`` can store more than one surface. But only one of each type, i.e., when ``SurfaceResource::get_surface`` and ``SurfaceResource(surface)``  is called, it stores the surface in a map where ``surface->token`` is the key. ``surface->token`` is like a string used to distinguish/name surfaces of different types. Token is static for each surface.

Conversion is mainly done by ``SurfaceResource::get_surface``. It takes multiple arguments, but its main job is to attempt to convert any available surfaces from the map into the requested surface type. It stores the conversion in the same map. When a lock is created, it converts the passed resource to the type argument and stores it.

This pattern of using tokens to distinguish between types and convert from one to another can be seen multiple times in Synfig. See :ref:`renderer_tasks`; tasks use a similar pattern.
