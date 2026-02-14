.. _renderer_optimizers:

Optimizers
==========

Optimizers can be found in ``synfig-core/src/synfig/rendering/common/optimizer``. They derive from the base class ``Optimizer``.

Renderer::optimize
~~~~~~~~~~~~~~~~~~

This function is responsible for running all the optimizations. It needs to take care of many things like handling changes to the ``Task::List`` by optimizers. The overall functions pseudocode looks like this:

.. code-block:: cpp


    Task::List list; // task list to optimize

    while(categories_to_process & ALL_CATEGORIES) // each category has an ID, which is used to create its bitmask. 1 << CATEGORY_ID and all ones is ALL_CATEGORIES
    {
        // this doesnt have to be while loop, since prepared_category >= current_category - 1 always
        // but it is while in code so I kept it like this
        while(prepared_category_id < current_category_id)
        {
            switch(++prepared_category_id) {
                // if theres some step required before running the categories optimizers do it here
                // example:
                case SPECIALIZED:
                    specialize(list); break; // specialize tasks before running optimizers which work on specialized tasks
            }
        }

        // check if we need to process this category, if not then skip
        if(!((1 << current_category_id) & categories_to_process))
        {
            // reset indexes
            optimizer_index = 0;
            current_category_id++;
        }

        Optimizer::List optimizers; // list of optimizers to run, depending on whether this category allows simultaneous run or not, if is a list of multiple optimizers or just one

        // run all for_list optimizers
        for(auto opt in optimizers)
        {
            if(opt->for_list)
            {
                opt->run(params);
            }
        }

        // for_task are recursive unless specefied by the task after running once
        bool nonrecursive = false;

        // run all for_task/for_root_task optimizers
        for(auto task in list)
        {
            Optimizer::RunParams params; // create params from list 
            Renderer::optimize_recursive(optimizers, params, for_root_task ? 0 : nonrecursive ? 1 : INT_MAX); // only let it run recursively if optimizer wants
            nonrecursive = false;

            task = params.ref_task;
            if((task.ref_mode & Optimizer::MODE_REPEAT_LAST) == Optimizer::MODE_REPEAT_LAST)
            {
                // dont go next
                if(!(task.ref_mode & Optimizer::MODE_RECURSIVE)) nonrecursive = true;
            }

            if(!params.ref_task) remove_from_list(task); // and dont go next

            categories_to_process |= params.ref_affects_to; // optimizer can ask to re run a category, it does that by setting ref_affects_to
            // only go next if the optimizer does not want to repeat optimization
        }

        optimizer_index += optimizers.size();
    }

    remove_dummy(list);

Renderer::optimize_recursive
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This function is responsible for running the optimizers on the task and its subtasks. It executes 4 main steps,

* Call non-deep-first optimizers
* Create a ``jump`` array, where each index stores index to next non-null sub task
* While there is a sub task to optimize
    * for each sub task in ``jump``
        * Call ``optimize_recursive`` on each subtask in ``jump``
        * Merge the result to ``params``, like ``ref_affects_to``
        * Remove sub task from ``jump``, unless optimizer tells to repeat
* Call deep-first optimizers

It uses a ``ThreadPool::Group`` to run ``optimize_recursive`` on subtasks in parallel.
