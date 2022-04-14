.. _ideas:

Ideas List
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a contributor, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Contributors are also welcome to propose their own project ideas.

Projects Ideas
--------------

User Interface improvements (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

Synfig's GUI lacks some of the well-used and intuitive features found in other programs. We want to improve user experience. In case

**- Expected Results**

Next issues should be resolved:

- Change mouse cursor while repositioning canvas (`#2036 <https://github.com/synfig/synfig/issues/2036>`_)
- Add ability to copy/paste waypoints (`#2139 <https://github.com/synfig/synfig/issues/2139>`_)
- Ctrl + tab to switch tabs (`#1936 <https://github.com/synfig/synfig/issues/1936>`_)
- Shortcuts for switching between animation windows (`#1422 <https://github.com/synfig/synfig/issues/1422>`_)
- Better way to set playback area (`#1493 <https://github.com/synfig/synfig/issues/1493>`_)
- Search functionality in properties panel (`#1048 <https://github.com/synfig/synfig/issues/1048>`_)
- Show/Hide rulers (`#1077 <https://github.com/synfig/synfig/issues/1077>`_)
- Add the possibility to change parameters using the mouse (`#2528 <https://github.com/synfig/synfig/issues/2528>`_)

**- Difficulty** Medium

**- Skills required** C++, gtkmm (recommended)

**- Mentor(s)** Artem Konoplin (https://github.com/ice0), Konstantin Dmitriev (https://github.com/morevnaproject)

Enhance building process by integrating Conan C++ package manager (175 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

Integrate Conan C/C++ package manager (https://conan.io/) to provide all required dependencies for building Synfig on any platform. See this page for details - https://github.com/synfig/synfig/issues/666

**- Expected Results**

Developers can easily set up build environment on any platform and any toolchain.

**- Difficulty** Easy

**- Skills required** Basic Linux system administration skills, Python, Bash scripting, Git, Conan, Docker (recommended), CMake (recommended)

**- Mentor(s)** Konstantin Dmitriev (https://github.com/morevnaproject)


Building Synfig with the MSVC/vcpkg toolkit (175 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

There are still some issues when building Synfig with the Microsoft Visual Studio/vcpkg toolchain.

**- Expected Results**

- Synfig is buildable with MSVC
- Add MSVC build to Github CI
- Fix issues when building Synfig with CMake Unity build

**- Difficulty** Easy

**- Skills required** C++

**- Mentor(s)** Artem Konoplin (https://github.com/ice0), Konstantin Dmitriev (https://github.com/morevnaproject)



Replacement of deprecated Gtk classes (175 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**- Description**

- get rid of deprecated Gtk::StockId (since before 2013)
- convert deprecated Gtk::Action to Gio::Action (since 2016)
- convert/get rid of deprecated Gtk::UIManager to Gtk::Builder (since 2013)

**- Expected Results**

Let Synfig Studio code avoid unmaintained code, as they can led to stability and security issues.
Besides, it will ease porting of Synfig Studio to upcoming Gtk 4, that deletes all currently deprecated classes and methods.

Synfig compiles with the `-DGDK_DISABLE_DEPRECATED -DGTK_DISABLE_DEPRECATED` flags on Ubuntu 16.04 and Ubuntu 20.04

**- Difficulty** Easy

**- Skills required*** C++, Gtkmm

**- Mentor(s)** Rodolfo Ribeiro Gomes (https://github.com/rodolforg), Ankit Kumar Dwivedi (https://github.com/ankit-kumar-dwivedi)


Performance enhancements (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**- Description**

Improve the speed of Synfig export to files and rendering speed.

If you want to learn about how to find bottlenecks in an application, especially in a complicated one like Synfig, and fix them - select this project :)

This project is *only for Linux* (Linux has the necessary tools)

**- Expected Results**

Improved Synfig rendering speed, allowing users to work faster and create more complex animations.

**- Difficulty** Medium

**- Skills required** C++ (perf optional)

**- Mentor(s)** Artem Konoplin (https://github.com/ice0), Konstantin Dmitriev (https://github.com/morevnaproject)

Optimization of internal operations with layers (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

The goal of this task is to introduce caching mechanism to allow Synfig work faster with layers and their parameters. 

There are two places where it is possible to introduce cache optimizations: 

1. The "set_time()" function. In curent implemetation, the "set_time()" is called every time when Synfig retrieves value of any layer parameter. This function recalculates parameters of all layers for specific moment of time, and writes them directly to layer objects. This is a slow operation and also leads to some bugs when using TimeLoop layer, time shift feature for groups and exported canvases. Instead of writing values into layer objects every time, it would be better to cache them.

2. Conveting layers into rendering tasks. When rendering process started, it reads layers tree and converts it into tree of "tasks" (structures, understandable by render engine). When any change is made to document, Synfig have to re-render everything again, so it reads full tree again and makes new set of tasks. When document have many layer, then this process takes much time and even with a small change to single parrameter it repeats the whole process (reads full layers tree). This process can be optimized by introducing an algorithm, which analyzes the change made to layers (their parameters) and propagates this change to the tree of rendering tasks.

Depending on available time and project size, aspiring contributor can choose to implement only first issue or both.

**- Expected Results**

Synfig will work faster on complex animations with many layers.

**- Difficulty** Medium

**- Skills required** C++

**- Mentor(s)** Anish Gulati (https://github.com/AnishGG), Ankit Kumar Dwivedi (https://github.com/ankit-kumar-dwivedi)


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
