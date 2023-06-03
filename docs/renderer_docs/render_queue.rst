.. _renderer_queue:

Renderer and Render Queue
=========================

A renderer in Synfig is resposible to take a ``Task::List``, optimize it, specialize the tasks and then run them. Renderers apply different optimizations and settings on the tasks. For example, the LowRes SW renderer changes various settings like resolution, blur type, etc. to make the render faster. The Safe SW Renderer does no optimizations, so its slower than other renderers.

The renderer then sends the optimized and specialized task list to the Render Queue.

Renderer
~~~~~~~~

The renderer is selected by the user from Synfig Studio UI or by passing a CLI arguement.

Each renderer has some modes registers, these modes are used for specializing tasks. For example, a software renderer will register the ``TaskSW::mode_token`` like so,

.. code-block:: cpp
    
    register_mode(TaskSW::mode_token.handle()); // function in Renderer class

Renderers derieve from the ``Renderer`` class, which has most of the functionality already built in. So, creating a new renderer is as simple as,

* derieve from ``Renderer`` class,
* override ``get_name()``,
* register optimizers and mode in the constructor,
* register renderer in ``Renderer::initialize_renderers``.

Renderer::enqueue
-----------------

This function takes a ``Task::List list`` and a ``TaskEvent finish_event``. It then makes ``finish_event`` dependent on every task in the ``list``, so once all the tasks in the ``list`` are finished executing, the ``finish_event`` task is ran and it signals the rendering as finished.

Before inserting the ``finish_event`` into the list, this function calls two very important functions ``optimize`` and ``find_deps``.

Renderer::Optimize
------------------

This function changes the ``list`` by adding/removing/modifying tasks to improve the render times. More details in :ref:`render_optimizers`.

Renderer::find_deps
-------------------

Render Queue
~~~~~~~~~~~~
