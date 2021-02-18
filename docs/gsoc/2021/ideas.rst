.. _ideas:

Ideas List
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a student, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Students are also welcome to propose their own project ideas.

Propose Project
---------------
If you have a project idea, edit the "Project Ideas" section below by filling the required details and sending a pull request (this page is editable at  https://github.com/synfig/synfig-docs-dev/blob/master/docs/gsoc/2020/ideas.rst), even if you could not mentor (we will find a mentor).

**Required information for project proposal**

::

    A descriptive title
    ~~~~~~~~~~~~~~~~~~~
    **- Description**

    A brief description about the project

    **- Expected Results**

    What benefit this deliver?

    **- Difficulty** Easy | Medium | High

    **- Skills required*** Knowledge Prerequisite

    **- Mentor(s)** Put your name if you are willing to mentor + other mentors.

*Please mention the following as comment on your proposal pr*

:Your name: :)
:Your profile: github | linkedin | etc 
:Your role: I am a making this proposal as a <student | mentor | community member | contributor | etc>

Projects Ideas
--------------

Move ETL library code to synfig-core
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

ETL is an Extended Template Library, like an extension of STL (Standard Template Library)
For those who don’t know. Synfig began to be developed in the early 2000s, when there was no C++11, multithreaded programming, and other things that had already become part of the standard (std).
The current task is to understand the logic of classes in ETL and replace them with an analogue from std, or if there are no analogs, transfer these classes as-is to synfig-core.

**- Expected Results**

From 3 packages (ETL, synfig-core, synfig-studio) 2 remains (synfig-core, synfig-studio)

**- Difficulty** Medium

**- Skills required** C++

**- Mentor(s)** Artem Konoplin (https://github.com/ice0), Ankit Kumar Dwivedi (https://github.com/ankit-kumar-dwivedi)


Replacement of deprecated Gtk classes
~~~~~~~~~~~~~~~~~~~
**- Description**

- get rid of deprecated Gtk::StockId (since before 2013)
- convert deprecated Gtk::Action to Gio::Action (since 2016)
- convert/get rid of deprecated Gtk::UIManager to Gtk::Builder (since 2013)
- (possibly) convert deprecated Gtk::Main 2 to Gtk::Application (since 2012)

**- Expected Results**

Let Synfig Studio code avoid unmantained code, as they can led to stability and security issues.
Besides, it will ease porting of Synfig Studio to upcoming Gtk 4, that deletes all currently deprecated classes and methods.

Synfig compiles with the `-DGDK_DISABLE_DEPRECATED -DGTK_DISABLE_DEPRECATED` flags on Ubuntu 16.04 and Ubuntu 20.04 

**- Difficulty** Easy

**- Skills required*** C++, Gtkmm

**- Mentor(s)** Rodolfo Ribeiro Gomes (https://github.com/rodolforg), Ankit Kumar Dwivedi (https://github.com/ankit-kumar-dwivedi)


Performance enhancements
~~~~~~~~~~~~~~~~~~~~~~~~
**- Description**

Improve the speed of Synfig export to files and rendering speed.

If you want to learn about how to find bottlenecks in an application, especially in a complicated one like Synfig, and fix them - select this project :)

This project is *only for Linux* (Linux has the necessary tools)

**- Expected Results**

Improved Synfig rendering speed, allowing users to work faster and create more complex animations.

**- Difficulty** Medium

**- Skills required** C++ (perf optional)

**- Mentor(s)** Artem Konoplin (https://github.com/ice0), Konstantin Dmitriev (https://github.com/morevnaproject)


Contacts
--------

https://www.synfig.org/contact/
