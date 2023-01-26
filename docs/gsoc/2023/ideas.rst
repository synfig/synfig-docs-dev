.. _ideas:

Ideas List
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a contributor, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Contributors are also welcome to propose their own project ideas.

Projects Ideas
--------------

Performance optimization (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**

| Improve Synfig rendering speed.
| If you want to learn about how to find bottlenecks in an application, especially in a complicated one like Synfig, and fix them - select this project :)

**Requirements: Linux (perf) / Windows (MSVC)**

- add flag to synfig CLI binary for performance test (it should measure the time taken to render the file, and allow to set the number of repeats)
- optimize rendering of the Solid Color layer
- test and optimize standard layers (circle, rectangle, gradient, etc)
- using multithreading to render the Solid Color layer
- fix the issue with creating unnecessary render threads
 
**Where to begin:**

Calculate the theoretical number of operations required to draw a Solid Color layer (without blending). Find why the Solid layer in practice spends more operations than in theory.

**Expected results:**

- At least 4 standard layers are optimized and the performance is close to theoretical (175 hours)
- At least 8 standard layers are optimized and the performance is close to theoretical (350 hours)

**Difficulty:** Medium/High

**Skills required:** C++

**Planned mentor(s):** Ayush Chamoli, `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_

macOS app bundle (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Requirements:** macOS

- add script/program to collect executable/library dependencies (python/c++ preferred) to SynfigStudio.app folder
- add support for signing binary files (this should be done in reverse order, files without dependencies should be signed first, SynfigStudio.app should be signed last)
- remove the macOS launcher script, add the code to set up the required macOS environment from the synfig/synfigstudio apps
- add cpack support to build installer on macOS
- add python and lxml packaging to .app (with signing))
- interface/menu improvements for more native macOS support

**Where to begin:**

- create prototype script/program to collect executable/library dependencies

**Expected results:**

- CMake/CPack builds SynfigStudio.app ready for distribution (175 hours)
- Synfig Studio is better adopted to macOS guidelines (350 hrs)

**Difficulty:** Medium

**Planned mentor(s):** `Dhairya Bahl <https://github.com/DhairyaBahl>`_, `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_

Optimization of internal operations with layers (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**

The goal of this task is to introduce caching mechanism to allow Synfig work faster with layers and their parameters.

There are two places where it is possible to introduce cache optimizations:

1.  The “set_time()” function. In current implementation, the “set_time()” is called every time when Synfig retrieves value of any layer parameter. This function recalculates parameters of all layers for specific moment of time, and writes them directly to layer objects. This is a slow operation and also leads to some bugs when using TimeLoop layer, time shift feature for groups and exported canvases. Instead of writing values into layer objects every time, it would be better to cache them.
2.  Converting layers into rendering tasks. When rendering process started, it reads layers tree and converts it into tree of “tasks” (structures, understandable by render engine). When any change is made to document, Synfig have to re-render everything again, so it reads full tree again and makes new set of tasks. When document have many layer, then this process takes much time and even with a small change to single parameter it repeats the whole process (reads full layers tree). This process can be optimized by introducing an algorithm, which analyzes the change made to layers (their parameters) and propagates this change to the tree of rendering tasks.

Depending on available time and project size, aspiring contributor can choose to implement only first issue or both.

**Expected Results:**

- Synfig will work faster on complex animations with many layers.

**Difficulty:** Medium

**Skills required:** C++

**Planned mentor(s)** `Anish Gulati <https://github.com/AnishGG>`_, `Ankit Kumar Dwivedi <https://github.com/ankit-kumar-dwivedi>_


Propose a Project
------------------
If you have a project idea, edit the "Project Ideas" section below by filling the required details and sending a pull request (this page is editable at  https://github.com/synfig/synfig-docs-dev/blob/master/docs/gsoc/2022/ideas.rst), even if you could not mentor (we will find a mentor).

**Required information for project proposal**

::

    A descriptive title (175 or 350 hrs)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    **- Description**

    A brief description about the project

    **- Expected Results**

    What benefit this deliver?

    **- Difficulty** Easy | Medium | High

    **- Skills required** Knowledge Prerequisite

    **- Mentor(s)** Put your name if you are willing to mentor + other mentors.

*Please mention the following as comment on your proposal pr*

:Your name: :)
:Your profile: github | linkedin | etc
:Your role: I am a making this proposal as a <student | mentor | community member | contributor | etc>

Contacts
--------

https://www.synfig.org/contact/
