.. _ideas:

Ideas List 
Just a check
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a student, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Students are also welcome to propose their own project ideas.

Propose Project
---------------
If you have a project idea, edit the "Project Ideas" section below by filling the required details and sending a pull request (this page is editable at  https://github.com/synfig/synfig-docs-dev/blob/master/docs/gsoc/2019/ideas.rst), even if you could not mentor (we will find a mentor).

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

Export for Web
~~~~~~~~~~~~~~

**- Description**

Allow to export animation for web through Lottie export format (http://airbnb.io/lottie/). See this page for details - https://github.com/synfig/synfig/issues/704

**- Expected Results**

Synfig becomes a platform for creating animated web content.

**- Difficulty** Medium

**- Skills required** C++ or Python,

**- Mentor(s)** Konstantin Dmitriev (https://github.com/morevnaproject)


Editable animation curves in Graphs Panel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**- Description**

A possibility to edit animation curves in Graphs Panel, in the same way as we can do that with bezier curves on workarea. See this page for details - https://github.com/synfig/synfig/issues/267

**- Expected Results**

Editing animation curves is a must-have feature for professional animators. Implementing this will get Synfig closer to professional animation tool.

**- Difficulty** Easy

**- Skills required** C++, GTKmm

**- Mentor(s)** Ivan Mahonin (https://github.com/blackwarthog)


Skeleton Tool
~~~~~~~~~~~~~

**- Description**

Currently the skeleton is created by adding a Skeleton Layer (via "Layer"->"New Layer"->"Other"->"Skeleton") and adding bones one-by-one through context menu. This is tedious and non-intuitive. It would be better to construct Skeleton in the same way as we can construct splines (using Spline Tool). So the proposal is to add Skeleton Tool, which allows user to "draw" bones and set their parent-child relationships.

**- Expected Results**

Simpler and faster Skeleton construction. Creating bones will be intuitively visible for new users (exposed as tool in Toolbox).

**- Difficulty** Medium

**- Skills required** C++, GTKmm

**- Mentor(s)** Ivan Mahonin (https://github.com/blackwarthog)

Vectorization of bitmaps
~~~~~~~~~~~~~~~~~~~~~~~~
**- Description**

Allow to convert bitmap images to vector data with a single command, through integration with Potrace (http://potrace.sourceforge.net/). Alternative - instead of Potrace, use an algorithm of vectorization from OpenToonz (https://opentoonz.readthedocs.io/ja/latest/drawing_animation_levels.html#converting-raster-drawings-to-vectors), which works better.

**- Expected Results**

User can quickly convert any bitmap image to vector and get resolution-independent image, available for edit.

**- Difficulty** Easy (Potrace) - Medium (OpenToonz)

**- Skills required** C++

**- Mentor(s)** Ivan Mahonin (https://github.com/blackwarthog)

Implement generic waypoint editing operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

Allow user to manipulate multiple waypoints in TimeTrack Panel. See this page for details - https://github.com/synfig/synfig/issues/266

**- Expected Results**

This will greatly simplify process of tweaking animation for user.

**- Difficulty** Medium

**- Skills required** C++

**- Mentor(s)** Ivan Mahonin (https://github.com/blackwarthog)

Simplify building process by utilizing Conan C++ package manager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**- Description**

Integrate Conan C/C++ package manager (https://conan.io/) to provide all required dependencies for building Synfig on any platform. See this page for details - https://github.com/synfig/synfig/issues/666

**- Expected Results**

Developers can easily set up build environment on any platform and any toolchain.

**- Difficulty** Easy

**- Skills required** CMake, Autotools, MSYS2

**- Mentor(s)** Konstantin Dmitriev (https://github.com/morevnaproject)

OpenToonz importer
~~~~~~~~~~~~~~~~~~

**- Description**

Synfig at its current status is not good for frame-by-frame animations. But it is good for morphing vectors, cut-out and motion design. With the ability to import OpenToonz files (which is good for vector frame-by-frame animation), Synfig users can get best of two worlds and use those applications together.

**- Expected Results**

Synfig users will be able to use frame-by-frame animations created in OpenToonz.

**- Difficulty** High

**- Skills required** C++ (and maybe Python)

**- Mentor(s)** Konstantin Dmitriev (https://github.com/morevnaproject)

Text Layer rewrite
~~~~~~~~~~~~~~~~~~~~

**- Description**

Current implementation of Text Layer uses old rendering engine, which makes it really slow. The task is to rewrite the Text Tool for new rendering engine, with consideration of solving its current issues - https://github.com/synfig/synfig/labels/Text

**- Expected Results**

A usable Text Tool in Synfig.

**- Difficulty** High

**- Skills required** C++, Freetype

**- Mentor(s)** Ivan Mahonin (https://github.com/blackwarthog)



Contacts
--------

https://www.synfig.org/contact/
