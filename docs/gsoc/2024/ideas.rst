.. _ideas:

Ideas List
=====================


This year we plan to apply to Google Summer of Code. Currently we are looking for project ideas. If you are a contributor, you are welcome to explore existing project ideas towards the GSoC application phase. There are ways to reach out to mentors, and many projects have lists of newcomer friendly issues you can start from. Contributors are also welcome to propose their own project ideas.

Projects Ideas
--------------

Automated release notes generator and packaging script fixes (175 or 350 hrs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description:**
We use Conventional commits (https://www.conventionalcommits.org/en/v1.0.0/) for git commits, so we can automatically split messages into groups (bug fixes, features, etc.)

- Add script which extract git messages starting from `start_commit_id` to `commit_id` (this can be achieved by using formatted `git log` output)
- Group these messages by type and output them in HTML/markdown format
- Add this script to CI pipeline (so it will show to user  how it changes final changelog) (optional)

**AppImage bundler:** We are currently using AppImage to prepare bundle for Linux, but we are using AppImage 1.0 which is outdated and needs to be updated.

- Write a script for packaging AppImage (there are ready-made scripts for packaging AppImage, but you need to check if they work correctly)

**Where to begin:**

Try extracting a list of messages from the git log output.

**Expected results:**

- Changelog file can be generated automatically
- AppImage bundle can be generated automatically

**Difficulty:** Easy/Medium

**Skills required:** Python/C++

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
