.. _renderer_tasks:

Layers and Tasks
================

Synfig documents are made up of layers. Synfig supports many different types of layers. Layers also have properties like opacity and z-depth. These are important when rendering. The layers are first sorted based on their depth and then rendered.

All the visual information under a layer is called its ``Context``.

Context
~~~~~~~

See file ``synfig-core/src/synfig/context.cpp``.

In code, ``Context`` is a const iterator over a list of layers. So it supports operators like ``++`` and ``--``. When ``Canvas::build_rendering_task`` is called, it first creates a sorted list of layers: the Context. Then it calls ``Context::build_rendering_task`` on the ``Context``. ``Context::build_rendering_task`` finds the first active layer, and calls ``Layer::build_rendering_task``, and sends ``context.get_next()`` as the Context for the layer.

``Layer::build_rendering_task`` calls ``Layer::build_rendering_task_vfunc``, this function is overridden by layers. It returns an **abstract** task for rendering this layer and everything below it(Context).

Tasks
~~~~~

See file ``synfig-core/src/synfig/rendering/task.cpp``.

Synfig has multiple engines, and not all tasks are compatible with every engine. For example, a blur task would differ for OpenGL and software rendering. Therefore, tasks need to be specialized based on the engine used. 

Abstract tasks are converted to Real tasks before running. Abstract tasks are used just for defining the overall task list. In contrast, Real tasks are the actual implementation of Abstract tasks. For example, the clamp task is implemented using an Abstract task ``TaskClamp`` and a Real task ``TaskClampSW``.

.. code-block:: cpp

    rendering::Task::Token TaskClamp::token(
        DescAbstract<TaskClamp>("Clamp") );
    rendering::Task::Token TaskClampSW::token(
        DescReal<TaskClampSW, TaskClamp>("ClampSW") );


``DescAbstract`` tells the token that it is a token for an Abstract task. Whereas ``DescReal`` tells it that it is a token for a Real task ``TaskClampSW``, which implements the Abstract task ``TaskClamp``.

Token
~~~~~

Before we understand the conversion, we need to understand Tokens in Synfig. Class ``Token`` in Synfig is used mainly for identification. It is used to create sort of an internal language for storing the different types used in Synfig.

The ``Token`` class is a doubly linked list. It has ``static`` members for the first and last token. The linked list operations are handled in the Constructor and Destructor. It also stores information like parents and all parents. Tokens also have a state ``prepared``, valid only after ``prepare_vfunc`` is called. ``prepare_vfunc`` is used for extra initialization done by any derived class.
For example, ``Task::Token`` has a map called ``alternatives_``. This map is filled when ``Task::Token.prepare_vfunc`` is called.

This initialization is started by ``Token::rebuild``, called by ``Synfig::Main``.

In most cases, classes have a synfig static member variable called ``token``, which is redeclared by every derived class. You will also see that these classes have a virtual function ``get_token``, which is implemented by derived classes and returns the redeclared token variable.

The base class ``Token`` doesn't store any information. The derived classes usually store more information; for example, they can store information for converting between types.

Task::Token
-----------

``Task::Token`` derieves from both ``Synfig::Token`` and ``Task::DescBase``. ``Task::DescBase`` is a class that stores some function pointers and other information useful during specialization. There are function pointers for creating, cloning, and converting. ``DescAbstract``, ``DescReal``, etc., are derived classes from ``DescBase``, and they assign the function pointers using the type arguments.

Looking at the ``TaskClamp`` example again, the code above initializes ``TaskClamp::token`` using a ``DescAbstract<TaskClamp>``. An abstract can be created and cloned but not converted from another task. So, the ``convert`` function pointer is null, whereas the ``create`` function pointer is set using ``DescBase::create_func`` and the ``clone`` function pointer is set using ``DescBase::convert_func`` which copies in a case like this. ``mode`` is assigned an empty handle for abstract tasks.

Then it initializes ``TaskClampSW::token`` using a ``DescReal<TaskClampSW, TaskClamp>``, which sets the convert function pointer using ``DescBase::convert_func``. It stores ``TaskClamp::token`` in ``abstract_task`` member variable. ``mode`` is assigned value of ``TaskClampSW::mode_token.handle()``. Then the most important step is done, in ``prepare_vfunc`` of ``Task::token`` if the task is a Real task, then its abstract task's ``alternative_`` map is filled, i.e., ``abstract_task.alternatives()[mode] = _Handle(*this)``. The abstract task is ``TaskClamp`` in this case. ``mode`` is explained in the next section.

Then an abstract task can be easily converted to a Real task given a mode by using ``alternatives_[mode]->convert(*this)``. ``alternatives_[mode]`` is storing the token of ``TaskClampSW`` in this example.

Specialization
~~~~~~~~~~~~~~

Tasks in Synfig are specialized based on ``mode``. Currently, in Synfig, there is only one mode ``TaskSW`` because there is only a software renderer. Each rendered has some modes associated with it. This gives users the ability to run only software or hardware tasks. It's not like hardware tasks cannot work with software tasks. They can work, which is possible due to automatic surface conversion. But it's faster if dependent tasks are of the same mode.

So, how does a renderer know which task runs on which mode? Real tasks derive from the ``Mode`` class, which stores a static token for its type and some additional functions which help the renderer. Now instead of deriving directly from the ``Mode`` class, software tasks derive from the ``TaskSW`` class, which derives from the ``Mode`` class. This is so that the mode token is the same for every software task.

Renderers register the mode they work on using the ``register_mode`` function. A software renderers calls ``register_mode(TaskSW::mode_token.handle())``. A renderer uses the registered modes to specialize tasks before sending them to the render queue.

Now that we know, what is required to implement a task, lets learn how to do it.

Implementation
~~~~~~~~~~~~~~

First, we need to create an Abstract task class. This will store all the properties necessary for executing the task.

.. code-block:: cpp

    class MyTask : public Task
    {
    public:
        typedef etl::handle<MyTask> Handle;
        static Token token;
        virtual Token::Handle get_token() const { return token.handle(); }

        // properties/settings
        float mul;

        // virtual functions as required or redeclare as required

        MyTask() : mul(0) {}
    }

Then we need to create its software implementation.

.. code-block:: cpp

    class MyTaskSW : public MyTask, public TaskSW
    {
    public:
        typedef etl::handle<MyTaskSW> Handle;
        static Token token;
        virtual Token::Handle get_token() const { return token.handle(); }

        virtual bool run(RunParams &params) const;
    }

Then we need to initialize ``MyTask::token`` and ``MyTaskSW::token`` in a cpp file.

.. code-block:: cpp

    rendering::Task::Token MyTask::token(
        DescAbstract<MyTask>("MyTask") );
    rendering::Task::Token MyTaskSW::token(
        DescReal<MyTaskSW, MyTask>("MyTaskSW") );

Implementation of ``run`` for ``ClampSW`` looks like,

.. code-block:: cpp

    bool
    TaskClampSW::run(RunParams&) const
    {
        RectInt r = target_rect;
        if (r.valid())
        {
            VectorInt offset = get_offset();
            RectInt ra = sub_task()->target_rect + r.get_min() + get_offset();
            if (ra.valid())
            {
                rect_set_intersect(ra, ra, r);
                if (ra.valid())
                {
                    LockWrite ldst(this); // lock target surface of this task
                    if (!ldst) return false;
                    LockRead lsrc(sub_task()); // lock target surface of sub_task, assumes only 1 sub task
                    if (!lsrc) return false;

                    const synfig::Surface &a = lsrc->get_surface();
                    synfig::Surface &c = ldst->get_surface();

                    for(int y = ra.miny; y < ra.maxy; ++y)
                    {
                        const Color *ca = &a[y - r.miny + offset[1]][ra.minx - r.minx + offset[0]];
                        Color *cc = &c[y][ra.minx];
                        for(int x = ra.minx; x < ra.maxx; ++x, ++ca, ++cc)
                            clamp_pixel(*cc, *ca);
                    }
                }
            }
        }

        return true;
    }


Special Tasks
~~~~~~~~~~~~~

There are some special tasks in Synfig, they do not do any processing but are used as utilities. Their tokens are created using ``DescSpecial``.

TaskSurface
-----------

**TODO**

TaskList
--------

This task is used to denote a list of tasks that need to executed sequentially.

TaskEvent
---------

This task is used for notifying when rendering has finished(using ``std::contidion_variable``). When ``TaskEvent::run`` is called, it signals that rendering has finished, atleast till the stage where this event was inserted.
