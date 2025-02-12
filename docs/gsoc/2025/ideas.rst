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



Synfig + controlnet-openpose integration (350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**
Project Idea:
Integration of a skeletal animation system into Synfig using ControlNet for generating images based on descriptions.

Problem:
Modern neural networks are capable of generating high-quality images, but achieving precise alignment with the author's vision requires additional control. ControlNet allows for specifying desired character poses, but the process of manually creating such poses can be labor-intensive, especially for animators who work with animation and require precision in details (e.g., foot placement, hand positions, or body rotation angles).

Solution:
1. **Adding a New Skeletal Layer to Synfig:**
   - Synfig will introduce a special layer containing a standard character skeleton. This skeleton will be integrated with ControlNet, allowing users to define character poses in an intuitive interface.
   - The layer will support character descriptions (e.g., "a mighty hero in adamantine armor"), which will be used for image generation.

2. **Image Generation via ControlNet:**
   - When the "Generate" button is clicked, Synfig will send skeleton data and text descriptions to the selected neural network (e.g., Stable Diffusion with ControlNet).
   - The neural network will generate an image, which will be automatically imported back into Synfig as a new layer (image?).

3. **Additional Features:**
   - To simplify the creation of animation sketches, a neural network that generates only character silhouettes can be integrated. This will allow for quick creation of basic poses for animation without the need for detailed rendering.
   - The ability to adjust the level of detail in the generated image (from silhouette to full render).

Advantages:
- Simplifying the animation process by automating character pose generation.
- Reducing the time spent on planning and rendering complex poses.
- An intuitive interface for animators, allowing them to focus on the creative aspects of their work.
- The ability to use the system for both full-fledged image creation and quick sketches.

Use Case:
An animator creates a character skeleton in Synfig, provides a description ("a knight in armor standing in a combat stance"), and selects a pose. After clicking the "Generate" button, the neural network creates an image of the knight in the specified pose, which is automatically added to the project. This allows the animator to focus on animation without spending time rendering each detail.

Future Development:
- The ability to import poses from other 3D animation software.
- Integration with other neural networks for generating backgrounds, textures, or additional elements.
- Adding support for custom skeletons and anatomical templates.

**Requirements:** Video adapter from NVidia/AMD/Intel with at least 8 GB of video memory, for running a local neural network. (need to check this)

**Where to begin:**
To begin with, it's necessary to see how ControlNet works and manually generate an image with the desired pose.
You can start with this website: https://huggingface.co/lllyasviel/sd-controlnet-openpose


**Expected outcome:**

- Added a new layer "OpenPose" with an additional property "Description", where the user can specify a description for the neural network.
- The "OpenPose" layer should contain a standard humanoid skeleton, with the ability to set the desired pose.
- Right-clicking on the "OpenPose" layer opens a menu with an additional "Generate" command.
- The "Generate" command sends the necessary data to the neural network server (this can be implemented by calling a local Python script that will handle the required task).
- After receiving a response from the script, the generated image is loaded into Synfig.
- A new tab should be added to the Synfig settings, containing settings for the neural network server.


**Difficulty:** Medium

**Skills required/preferred:** Python/C++

**Possible mentor(s):** `Mohamed Adham <https://github.com/mohamedAdhamc>`_ , `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_

**Expected size of project:** 350 hours



Plugin manager dialog (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**
A dialog for management of Synfig Studio plugins and Synfig modules.
The dialog would show the currently supported plugin/module metadata and allow user to:

- uninstall it
- open its folder in the default OS file manager
- install a plugin from a zip package already stored in local machine
- maybe browse/download from a (future) online plugin repository (maybe a dedicated service or a special session from Synfig forums or even a git server)

**Expected outcome:**

**175 hours**

- Users could easily list, install and uninstall plugins and modules they downloaded from Internet.

**350 hours**

- Plugin repository

**Difficulty:** Easy/Medium

**Skills required/preferred:** C++, gtkmm

**Possible mentor(s):** `Rodolfo Ribeiro Gomes <https://github.com/rodolforg>`_, `Mohamed Adhamc <https://github.com/mohamedAdhamc>`_

**Expected size of project:** 175 or 350 hours

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
