.. _renderer_queue:

Renderer and Render Queue
=========================

A renderer in Synfig is responsible to take a ``Task::List``, optimize it, specialize the tasks and then run them. Renderers apply different optimizations and settings on the tasks. For example, the LowRes SW renderer changes various settings like resolution, blur type, etc. to make the render faster. The Safe SW Renderer does no optimizations, so its slower than other renderers.

The renderer then sends the optimized and specialized task list to the Render Queue.

Renderer
~~~~~~~~

The renderer is selected by the user from Synfig Studio UI or by passing a CLI argument.

Each renderer has some modes registers, these modes are used for specializing tasks. For example, a software renderer will register the ``TaskSW::mode_token`` like so,

.. code-block:: cpp
    
    register_mode(TaskSW::mode_token.handle()); // function in Renderer class

Renderers derive from the ``Renderer`` class, which has most of the functionality already built in. So, creating a new renderer is as simple as,

* derive from ``Renderer`` class,
* override ``get_name()``,
* register optimizers and mode in the constructor,
* register renderer in ``Renderer::initialize_renderers``.

Renderer::enqueue
-----------------

This function takes a ``Task::List list`` and a ``TaskEvent finish_event``. It then makes ``finish_event`` dependent on every task in the ``list``, so once all the tasks in the ``list`` are finished executing, the ``finish_event`` task is ran and it signals the rendering as finished.

Before inserting the ``finish_event`` into the list, this function calls two very important functions ``optimize`` and ``find_deps``.

Renderer::Optimize
------------------

This function changes the ``list`` by adding/removing/modifying tasks to improve the render times. It also calls ``linearize`` and ``specialize``. More details in :ref:`render_optimizers`.

Renderer::find_deps
-------------------

This function fills ``deps`` and ``back_deps`` members of ``Task::RenderData`` for each task in the passed linearized task list. It first finds dependencies based on same target surface. Tasks are also depended on other tasks if their sub tasks share the target with the other task. Dependency direction is based on position in the linear list. Tasks that come later are dependend on the tasks that come earlier if they share taret.

It also removes dependencies between tasks if their target rect is non-overlapping.

Now sub tasks don't have the same target surface as their parent task. So by the logic above the parent task is not depended on the sub task, but in reality it should be. This is handled by ``Renderer::linearize``.

Renderer::linearize
-------------------

This function turns the tree of tasks into a linear list. Sub tasks are inserted before the parent task in the list, and are converted into ``TaskSurface`` in the parents ``sub_task`` list. Since sub tasks come before parent tasks in the list and the ``TaskSurface`` has the same target surface as the sub task, ``find_deps`` is able to find the dependency.

Render Queue
~~~~~~~~~~~~

This class is responsible for running tasks that support multithreading and those that don't. A separate thread is for running just the tasks which don't support multithreading. ``Renderer`` initializes a static ``RenderQueue``. It creates a set number of threads and calls ``process(thread_index)`` on each thread.

This class uses ``std::condition_variable`` for notifying other threads when new tasks are available. And it uses ``std::mutex`` when any changes are being made to the queue.

process
-------

This function picks up any task in the ``ready_tasks`` or ``single_ready_tasks`` (for tasks that don't allow multithreading). It does so by calling ``get()`` which waits on ``cond``. It then runs the task and calls ``done()`` after completion.

done
----

This function goes through all the tasks that depend on the completed task and then removes the completed task from their dependency. If no more dependencies exist, it inserts the task to ``ready_tasks`` or ``single_ready_tasks``. After that, it calls ``cond.notify_one()`` some times, which depends on the number of tasks added to ``ready_tasks``. A similar thing is done for ``single_cond``.
