Release procedure
=================

Preparation
~~~~~~~~~~~

All release procedures are done from "testing" branch. So, switch to "testing" branch first and then set it to the last commit you want to release from.

Before you start procedure, you have to make sure that current content of "testing" branch compiles fine on all supported platforms - Windows, Linux and OSX. If everything is alright, tthen proceed with the steps below.

Apply latest translations
~~~~~~~~~~~~~~~~~~~~~~~~~

#. Install transifex-client ('dnf install transifex-client' on Fedora)
#. Run 'bash ./autobuild/transifex-pull-translations.sh'
#. Fix lines marked as "TODO" in synfig-studio/src/languages.inc.c (if any)
#. Update and push translation template to transifex 'bash ./autobuild/transifex-push-template.sh'
#. Commit changes.
    
Update version number
~~~~~~~~~~~~~~~~~~~~~

From the root dir of synfig repository:

.. code:: bash

    $ ./release-bump.sh <VERSION_NUMBER>
    
i.e.:

.. code:: bash

    $ ./release-bump.sh 1.3.10
    
Changes are committed automatically.

Push to upstream
~~~~~~~~~~~~~~~~

.. code:: bash

    $ git push upstream testing


Build packages
~~~~~~~~~~~~~~

#. Build source tarballs - './autobuild/synfigstudio-release.sh'
#. Build OSX package and upload to deploy server.
#. Wait when build bot will finish building packages for Linux and Windows.

Write press release
~~~~~~~~~~~~~~~~~~~

Carefully examine commits history and write press release.

Example - https://www.synfig.org/2018/06/25/synfig-studio-1-3-9-released/

Finish repository changes
~~~~~~~~~~~~~~~~~~~~~~~~~

Add tag:

.. code:: bash

    git tag v1.3.10
    git push --tags upstream

(Optionally) merge into master branch:

.. code:: bash

    git checkout master
    git merge testing
    git push upstream


    
Publish packages
~~~~~~~~~~~~~~~~~

Login to deploy server and upload packages to

* GitHub
* FossHub
* SourceForge

Update Paddle products.

Publish announcement
~~~~~~~~~~~~~~~~~~~~

* Publish press release
* Patreon
* Notify subscribers via email newsletter
* VK
* Udemy

  * Announcement
  * Update required version in description/video/link (if needed)
  
* Indiegogo
* LWN.net

Close related bugs
~~~~~~~~~~~~~~~~~~~~

Visit https://github.com/synfig/synfig/projects/1 and move all bugs related to release in "Released" column.

Comment on those bugs about new release.
