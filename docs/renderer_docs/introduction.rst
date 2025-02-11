.. _renderer_intro:

Introduction
============

There are two ways to render a Synfig document, by using the **synfig** CLI tool or via **Synfig Studio**. The rendering code is the same, but some concepts would be easier explained using the CLI tool. A basic command to render a Synfig document using the CLI looks like this

.. code-block:: bash

   synfig $FILE -o out.png --time=0 --width=1920 --height=1080

This will render only the first frame(``--time=0``) of ``$FILE`` with dimensions 1920x1080 to target *out.png*. Synfig supports rendering to multiple file formats. These file formats are represented as ``Target`` in Synfig's code base. The CLI performs the following tasks to render an image:

* Boot Synfig by using ``synfig::Main``, which initializes different modules and systems.
* Read the document file and create the internal representation of the document.
* Extract and execute a ``Job``.

The first two steps are not part of the Renderer. Therefore this section only covers the last step.

Job
~~~

This class is present in CLI only, and it is simple. This class stores information used by other functions to start rendering the file. The most important fields it stores are ``synfig::RendDesc desc`` and a handle to the ``synfig::Canvas``. After all the initialization and reading of the document is done, the first step taken by the CLI is to create and fill a Job.

Usually, only one Job is created, but two jobs are created if the user wants to extract Alpha to another file. These jobs are first set up and run. The setup step attempts to find the ``Target`` specified by the user or by the file extension, Render Engine to use, permissions, etc.

To start rendering the file, ``job.target->render(..)`` is called. This function actually starts the rendering process.

Target
~~~~~~

``Target`` represents the output (file or memory) and handles the frame-by-frame rendering process. The base class ``Target`` has a few virtual methods overridden by derived classes like ``Target_Scanline``. Targets for output files are modules that derive mostly from ``Target_Scanline``. Details about how the correct ``Target`` class is acquired and the actual working of ``Target::render()`` can be found in :ref:`renderer_target_surface`.

The Cobra engine is multithreaded, executing independent Tasks on different threads. The function ``Canvas::build_rendering_task`` creates a Task for rendering a frame. This function is called by ``Target::render()``.

Tasks
~~~~~

Tasks are the main objects of the Cobra engine, which writes and transforms pixels. There are tasks for blending, rendering shapes, transformation, etc. Tasks can have dependencies stored in a ``sub_tasks`` list inside the class ``Task``. ``Canvas::build_rendering_task`` builds this graph of Tasks, which is then sent to the Render Engine for execution. Details on how this Task list is build can be found in :ref:`renderer_tasks`.

Renderer 
~~~~~~~~

A Renderer in Synfig receives a Task list, processes it, and runs it. Synfig has multiple renderers, like Draft SW, Preview SW, etc. Currently, there are only Software Renderers in Synfig. ``Tasks`` are specialized based on the chosen renderer. This is because Software Tasks can not be directly run using GPU. The base class ``Renderer`` does most of the work. It is responsible for Optimizing the Task List, constructing the Tasks, and then sending them to the Render Queue. More details in :ref:`renderer_queue`.

Render Queue
~~~~~~~~~~~~

There is a singleton Render Queue always waiting for new Tasks to run. It creates threads that are always waiting for new Tasks. More details in :ref:`renderer_queue`.
