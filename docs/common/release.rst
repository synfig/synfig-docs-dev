Release procedure
=================

Preparation
~~~~~~~~~~~

All release procedures are done from "testing" branch. So, switch to "testing" branch first and then set it to the last commit you want to release from.

Before you start procedure, you have to make sure that current content of "testing" branch builds succesfully on all supported platforms - Windows, Linux and OSX.

Also, make sure that source tarball generation script executes without errors:

.. code:: bash

    $ ./autobuild/synfigstudio-release.sh

Apply latest translations
~~~~~~~~~~~~~~~~~~~~~~~~~

#. Install transifex-client ('dnf install transifex-client' on Fedora)
#. Run 'bash ./autobuild/transifex-pull-translations.sh'
#. Fix lines marked as "TODO" in synfig-studio/src/languages.inc.c (if any)
#. Update and push translation template to transifex 'bash ./autobuild/transifex-push-template.sh'
#. Commit changes.

Update NEWS files
~~~~~~~~~~~~~~~~~~~~~~~~~

Summarize all changes in NEWS files.
    
Update version number
~~~~~~~~~~~~~~~~~~~~~

If you are doing (major) stable release, then make sure to edit following files:

* `synfig-core/src/synfig/version.h`
* `synfig-core/src/synfig/releases.h`
* `synfig-studio/src/gui/app.cpp`

Make sure the release number is correct. If it is not, then execute 'version-bump.sh' script from the root dir of synfig repository:

.. code:: bash

    $ ./version-bump.sh <VERSION_NUMBER>
    
i.e.:

.. code:: bash

    $ ./version-bump.sh 1.4.0
    
This will update all files and commit changes.

Next, execute 'version-release.sh' script:

.. code:: bash

    $ ./version-release.sh
    
It will add neccesary tags and


Build packages
~~~~~~~~~~~~~~

#. Build source tarballs - './autobuild/synfigstudio-release.sh'
#. Build MacOS package and upload to deploy server.
#. Wait when build bot will finish building packages for Linux and Windows.

Write press release
~~~~~~~~~~~~~~~~~~~

Carefully examine commits history and write press release.

Example - https://www.synfig.org/2018/06/25/synfig-studio-1-3-9-released/

Finish repository changes
~~~~~~~~~~~~~~~~~~~~~~~~~

Merge branch 'testing' into 'master':

.. code:: bash

    git checkout master
    git merge testing
    git push upstream

...And bump version number, so it will be next development release:

.. code:: bash

    $ ./version-bump.sh 1.5.0
    
Publish packages
~~~~~~~~~~~~~~~~~

Login to deploy server and upload packages to

* GitHub
* FossHub
* SourceForge

Update Paddle products.

Update flatpak and snap packages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**flatpak**

.. code:: bash

    git clone --recursive https://github.com/flathub/org.synfig.SynfigStudio.git
    # edit org.synfig.SynfigStudio.yaml and update packages
    flatpak-builder --user --install build-dir org.synfig.SynfigStudio.yaml
    # test build locally
    flatpak run org.synfig.SynfigStudio
    # make pull request with your changes
    
**snap**

.. code:: bash

    git clone https://github.com/synfig/synfig.git
    cd autobuild/snap-stable/
    # edit snapcraft.yaml and update packages
    # remote build using launchpad.net servers
    snapcraft remote-build
    # install snap and test it
    snap install --dangerous ./synfigstudio_1.4.0_amd64.snap
    # upload to snapcraft store
    snapcraft upload --release=candidate synfigstudio_1.4.0_amd64.snap
    # after test move it from candidate to release channel
    # make pull request with your changes
    

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
