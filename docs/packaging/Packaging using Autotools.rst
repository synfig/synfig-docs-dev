.. _Packaging using Autotools:

Packaging using Autotools
==========================

OSX
~~~

You can build OSX package by executing following 3 scripts from Synfig's source root:

.. code:: bash

    $ ./1-setup-osx-brew.sh
    $ ./2-build-production.sh
    $ ./3-package-osx-dmg.sh

Please refer to https://synfig-docs-dev.readthedocs.io/en/latest/common/building.html for details.

Windows and Linux
~~~~~~~~~~~~~~~~~

We have a set of scripts that allow to build Windows installers and Linux Appimages in isolated Docker environment. Both types of packages are built on Linux platform (Windows binaries are built using cross-compilation).

TL;DR;
------

1. Fetch builder repository:

.. code:: bash

    $ git clone https://github.com/morevnaproject/morevna-builds
    $ cd morevna-builds
    
2. Run builder:

.. code:: bash

    $ ./build-synfigstudio.sh
    
This will fetch Docker image, and build packages for Windows and Linux, 64bit and 32bit. This process involves building all required libraries, so it will take a very long time.

After build finishes grab packages from "publish" directory.


Resolving problems
------------------

It might happen that compilation fails at some stage. This could be for various reasons.

One very common reason could be that updates to builder scripts conflicting with what you already have built before (when using previous versions of this builder). Unfortunately, such cases are hard to track and the easiest workaround is to completely remove "docker-builder-data/build/packet" directory and re-run build script. This will build everything from scratch.

If the problem persists, please submit error message to https://github.com/morevnaproject/morevna-builds/issues

You can also try to fix problem by yourself by jumping into Docker's build environment of particular package (the one that failing). Depending on the platform and architecture you do that with specific command:

1. For Linux 64-bit:

.. code:: bash

    $ ./docker/debian-7-64bit/run.sh PACKAGENAME shell

2. For Linux 32-bit:

.. code:: bash

    $ ./docker/debian-7-32bit/run.sh PACKAGENAME shell
    
3. For Windows 64-bit:

.. code:: bash

    $ ./docker/debian-7-64bit/win64.sh PACKAGENAME shell
    
    
4. For Windows 32-bit:

.. code:: bash

    $ ./docker/debian-7-64bit/win32.sh PACKAGENAME shell
    
Make sure to replace **PACKAGENAME** with actual package name (script name from **docker-builder-data/build/script/packet**, without ".sh" at the end).

Directory structure
-------------------

* "docker-builder-data/build/packet" - directory where build files stored.
* "docker-builder-data/build/script/packet" - script files, which tell how to build each particular package.
* "docker/debian-7-32bit" - 32-bit Docker environment configuration and scripts.
* "docker/debian-7-64bit" - 64-bit Docker environment configuration and scripts.


