Building Synfig
===============

Synfig is written in C++ with GTK+ libraries.

Build system: autoconf/make

Things to know before you start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

TODO: Write here about structure of Synfig (3 components ETL, core, studio).

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

As alternative we can use a NIX build system, which will assist us in setting correct build environment.

First of all, install NIX using the following command:

.. code:: bash

    $ curl -L http://git.io/nix-install.sh | bash && source ~/.nix-profile/etc/profile.d/nix.sh

Now you can build Synfig:

.. code:: bash

    $ cd ~/synfig.git/autobuild/
    $ nix-build
    
When the build succeeds, it will create a symlink called "result" in the current directory, which will point to Synfig installation. So you can run it with the following command:

.. code:: bash

    $ ./result/bin/synfigstudio
    
HINT: If you want to make result of the build available in your PATH, then you can use the following command instead of  "nix-build":

.. code:: bash

    $ nix-env -i -f default.nix
    
In this case you can simply run the result as follows:

.. code:: bash

    $ synfigstudio


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

