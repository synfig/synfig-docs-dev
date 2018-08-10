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
    $ cd ~/synfig.git
    
Building using NIX
------------------------------------------

Building Synfig requires many dependent libraries installed for your system (see first chapter above). The installation of those libraries natively in your system have several disadvantages:

#. Installing the dependencies can be non-trivial - you have to figure out package names for each particular distro.
#. Different Linux distros have different ersions of required packages, which makes build process less predictable
#. Sometimes installing many development packages is not desirable, because it is cluttering your system.

So, we are using `NIX build system <https://nixos.org/>`_ to quickly generate development environment with all required dependencies.

First of all, install NIX using the following command:

.. code:: bash

    $ curl https://nixos.org/nix/install | bash
    $ source ~/.nix-profile/etc/profile.d/nix.sh
    
Now you can enter development environment, where all dependencies are available:

.. code:: bash

    $ cd ~/synfig.git/
    $ ./dev-env.sh
    
If you are running this command for first time, then you will have to wait while system fetches all required dependencies. Do not worry - those fetched packages won't affect the status of your base operating system.

When the process is done you will see a green prompt:

.. code:: bash

    $ [nix-shell:~/synfig.git/]$

That means you are in development environment now. Let's build Synfig!

We have a special script, which carries all build routines for you. In fact, there are two of them - "build-debug.sh" and "build-production.sh".

As you might guess, the first one is for building development version with debug symbols (useful for development itself) and the second one is without debug symbols (useful for production).

Another difference is that first script places result of the build in "_debug/build" subdirectory, and with second script the result will reside in "_production/build".

in all other aspects both scripts work exactly the same and accept the same arguments.

Since we are after developing Synfig, let's continue with first script - "build-debug.sh".

You can build everything by simply executing the script:

.. code:: bash

    $ ./build-debug.sh
    
The script will build and install ETL, then synfig-core and finally - synfig-studio.

When building is done, you can launch Synfig by executing

.. code:: bash

    $ ~/synfig.git/_debug/build/bin/synfigstudio
    
    
Re-building your changes
------------------------------------------

Of course it is not very convenient to run a full rebuild process on every change. So, the script provides a set of arguments that allow you to execute particular stages of the build:

The syntax is:

.. code:: bash

    $ ./build-debug.sh [package] [phase]
    
where:

[package] can have following values:

* all  - builds all three packages (default).
* etl - builds ETL only.
* core - builds synfig-core only.
* studio - builds synfig-studio only.
  
[phase] allows you to choose particular phase to execute for given package:

* clean - does "make clean" operation.
* configure - running "./configure" script with all neccessary options.
* make - running "make" command and "make install".
* build - executes "configure" and "make" phases (default).
* full - executes all phases: "clean", "configure" and "make" (exactly in that order).

You might ask: why execute those commands/phases from a script , while it is possible to call "./configure" and "make" commands by hand in particular directories? Well, for "make" this would work and is desirable for many cases. But for "./configure" you have to specify many parameters, such as prefix, and locations of some dependent libraries. So it is more convenient to call "./configure" using this helper script.

Examples:

1. Configure and (re)build synfig-core (executes "./configure", "make" and "make install"):

.. code:: bash
    ./build-debug.sh core
    
equivalent to:

.. code:: bash
    ./build-debug.sh core build

2. Do a full clean build of synfig-core (executes "make clean", "./configure", "make" and "make install"):

.. code:: bash
    ./build-debug.sh core full

3. Quick rebuild of synfig-core (without executing "./configure"):

.. code:: bash
    ./build-debug.sh core make
    
Since "make" doesn't require any parameters, the same result can be achieved by executing:

.. code:: bash
    cd ~/synfig.git/synfig-core/
    make install

4. Quick rebuild of of everything - ETL, synfig-core and synfig-studio (without executing "./configure"):

.. code:: bash
    ./build-debug.sh all make

Finally, some recommendations when to call particular phases.

Considering the structure of Synfig (see first chapter of this article), we have following dependency chain:

synfig-studio -> synfig-core -> ETL

So, you should follow this logic:

* when change is made to ETL, then rebuild everything - ETL, synfig-core and synfig-studio;
* when change is made to synfig-core, then you can rebuild synfig-core and synfig-studio only;
* when change is made to synfig-studio, then you need to rebuild synfig-studio only;

You might notice that if you rebuild simply by running "make install" that takes considerably less time than when you do a ful-cycle rebuild with "./configure" and then "make install".

So, when it is safe to skip "./configure"? 

The answer is: if you edited .h and .cpp files only, then it is safe to skip. In all other cases it is safer to re-start ./configure on rebuilding.

Let's suppose you made changes in synfig-studio and want to rebuild it without re

And finallly a quick note about "build.conf.sample" file in the root of source repository.

With this file you can tweak the number of threads used by the build scripts. Just copy "~/synfig.git/build.conf.sample" to "~/synfig.git/build.conf" and adjust its contents according to your needs.

Building on Windows
~~~~~~~~~~~~~~~~~~~~~~

Build using MinGW cross-compiler:

WRITEME

Building on OSX
~~~~~~~~~~~~~~~~~~~~~~

WRITEME

