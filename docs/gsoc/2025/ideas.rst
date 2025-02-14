.. _ideas:

Ideas List
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a contributor, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Contributors are also welcome to propose their own project ideas.

Projects Ideas
--------------

macOS app bundle (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**

We are currently using a script to create a macOS app bundle, but it has some issues.
Generation takes a long time (could be greatly improved), it uses incorrect paths in some places and does not sign files.

**Requirements:** macOS or access to macOS command-line (so you can test your script)

- add script/program to collect executable/library dependencies (python/c++ preferred) to SynfigStudio.app folder
- add support for signing binary files (this should be done in reverse order, files without dependencies should be signed first, SynfigStudio.app should be signed last)
- remove the macOS launcher script, add the code to set up the required macOS environment from the synfig/synfigstudio apps
- add cpack support to build installer on macOS
- add python and lxml packaging to .app (with signing))
- interface/menu improvements for more native macOS support

**Where to begin:**

- create prototype script/program to collect executable/library dependencies

**Expected outcome:**

**175 hours**

- CMake/CPack builds SynfigStudio.app ready for distribution

**350 hours**

- Synfig Studio is better adopted to macOS guidelines

**Difficulty:** Medium

**Skills required/preferred:** Python/C++

**Possible mentor(s):** `Dhairya Bahl <https://github.com/DhairyaBahl>`_, `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_

**Expected size of project:** 175 or 350 hours





Synfig Android Version (350hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**
This project aims at providing a solid ground for a Synfig Android version. It aims to do so through two main parts.

1- Prototype UI (Using Qt for android) that uses synfigapp and -in turn- synfig-core

There are two main goals here:
1. To have a basic android UI for synfig working. 
2. While making this prototype certain parts of the synfig api would be fixed. Which would make SynfigApp and Synfig-Core able to be used with any other UI not just the current gtkmm UI (synfig-studio).

2- Add more features to the UI
Synfig is quite a huge application. Most likely this app would start with only very basic needed synfig features added. Then gradually adding more features from synfig-studio to the new prototype UI.

**Where to begin:**

1. Start out by understanding and gathering the basic features for animation in synfig. In your proposal include these features and expand on how you plan to include them. 
2. Research the available mobile/tablet animation apps and prototype a ui design using any ui design software (e.g. canva). This is not required but it will definetly help your proposal.


**Expected outcome**

- Prototype Synfig Android Version
- Improved synfig-app and (possibly) synfig-core that can work with any other UI.

**Difficulty:** Medium/High

**Skills required/preferred:** C++, gtkmm, Qt, using Qt for Android

**Possible mentor(s):** `Mohamed Adham <https://github.com/mohamedAdhamc>`_ , `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_

**Expected size of project:** 350 hours


Brush tool (175hrs)
~~~~~~~~~~~~~~~~~~~

**Description:**

Synfig is primarily designed for vector-based animation, but it also supports the use of raster images within animations.
However, the current functionality only allows for the use of raster images imported from external files (usually BMP, JPG or PNG), limiting users from drawing directly within the application.
The goal of this project is to implement the missing Brush tool for raster drawing, allowing users to draw raster content directly in the app.
An early attempt to implement this feature, called 'Brush,' exists, but it is entirely nonfunctional. Users are unable to make even a single stroke with the tool.

**Where to begin:**

1. Look for the code of how tools are implemented in Synfig. As they are coded as a finite state machine, the correspondent files are name as state_
2. `synfigapp` is responsible for handling the interface between the graphical user interface (GUI) and the underlying core engine of Synfig (which handles the animation and rendering processes).
There are some synfigapp::Actions trying to implement it, as in synfig-studio/src/synfigapp/actions/layerpaint.h

**Expected outcome**

A working tool that allows users to freely hand-draw their artwork, which can then be animated within Synfig, with undo/redo functionality while drawing and features like brush selection, coloring options, and erasing.

**Difficulty:** Medium/High

**Skills required/preferred:** C++, gtkmm, 2D-drawing

**Possible mentor(s):** `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_ , `Mohamed Adham <https://github.com/mohamedAdhamc>`_

**Expected size of project:** 175 hours

Propose a Project
------------------
If you have a project idea, edit the "Project Ideas" section below by filling the required details and sending a pull request (this page is editable at  https://github.com/synfig/synfig-docs-dev/blob/master/docs/gsoc/2025/ideas.rst), even if you could not mentor (we will find a mentor).

**Required information for project proposal**

::

    A descriptive title (175 or 350 hrs)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    **Description**

    A brief description about the project

    **Expected outcome**

    What benefit this deliver?

    **Difficulty** Easy | Medium | High

    **Skills required/preferred:** Knowledge Prerequisite

    **Possible mentor(s):** Put your name if you are willing to mentor + other mentors.

    **Expected size of project:** 90, 175 or 350 hours

*Please mention the following as comment on your proposal pr*

:Your name: :)
:Your profile: github | linkedin | etc
:Your role: I am a making this proposal as a <student | mentor | community member | contributor | etc>

Contacts
--------

https://www.synfig.org/contact/
