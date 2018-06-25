Building Synfig
===============

Synfig is written in C++ with GTK+ libraries.

Build system: autoconf/make

Things to know before you start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Synfig is divided into three components: ETL, synfig-core and synfig-studio.

* ETL - the extended template library.
* synfig-core - contains the render engine and core/command-line tool (cli) 
* synfig-studio - gui for the application.
    
All those three components are included with the main source repository of Synfig - `<https://github.com/synfig/synfig.git>`_.

ETL is required for synfig-core and synfig-core is required for building synfig-studio.

So, first builds ETL, then synfig-core, and then synfig-studio.

Besides that, synfig-core and synfig-studio require following libraries to be available in your system:

TODO: List dependencies. 

Building on Linux
~~~~~~~~~~~~~~~~~

Fetching sources
----------------

First of all, you need to get sources from GitHub:

.. code:: bash

    $ git clone https://github.com/synfig/synfig.git ~/synfig.git
    
Build approach #1 - recommended
--------------------------------------

We provide a special build script which installs all required packages for your distro (most popular are supported) and builds Synfig source for you.

TODO: Write which distros are supported by build script.

.. code:: bash

    $ cd ~/synfig.git/autobuild/
    $ ./synfigstudio-linux-build.sh

After that you can find synfig installed in "~/synfig/". So you can run it:

.. code:: bash

    $ ~/synfig/bin/synfigstudio
    
Build approach #2 - using NIX (WIP)
------------------------------------------

The approach above have several disadvantages:

#. You have to install development packages, which is cluttering your base system
#. Sometimes new versions of distributives change development package names, which may causeautomatic script to fail.
#. It is possible your distributive is not supported by our build script.

As alternative we can use a `NIX build system <https://nixos.org/>`_, which will assist us in setting correct build environment.

First of all, install NIX using the following command:

.. code:: bash

    $ curl https://nixos.org/nix/install | bash
    $ source ~/.nix-profile/etc/profile.d/nix.sh

Now you can build Synfig:

.. code:: bash

    $ cd ~/synfig.git/autobuild/
    $ nix-build
    
When the build succeeds, it will create a symlink called "result" in the current directory, which will point to Synfig installation. So you can run it with the following command:

.. code:: bash

    $ ./result/bin/synfigstudio



Re-building your changes
--------------------------------------

WRITEME

Building on Windows
~~~~~~~~~~~~~~~~~~~~~~

Build using MinGW cross-compiler:

WRITEME

Building on OSX
~~~~~~~~~~~~~~~~~~~~~~

WRITEME

