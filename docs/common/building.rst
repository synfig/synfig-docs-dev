.. _building:

Building Synfig
===============

Synfig is written in C++, based on GTK3 library.

Build system: autotools/make

Things to know before you start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Synfig is divided into three components: ETL, synfig-core and synfig-studio.

* ETL - the extended template library.
* synfig-core - contains the render engine and core/command-line tool (cli) 
* synfig-studio - gui for the application.
    
All those three components are included with the main source repository of Synfig - `<https://github.com/synfig/synfig.git>`_.

ETL is required for synfig-core and synfig-core is required for building synfig-studio.

So, first builds ETL, then synfig-core, and then synfig-studio.

Preparing Environment
~~~~~~~~~~~~~~~~~~~~~~

Building Synfig requires many dependent libraries installed for your system. For full list of libraries please refer to :ref:`this page <dependencies>`.

Below you will find instructions how to install them on various operating systems.

Linux
-------

First of all make sure you have "git" installed. Use it to fetch Synfig's sources:

.. code:: bash

    $ git clone https://github.com/synfig/synfig.git ~/synfig.git
    
Then navigate to sources directory:

.. code:: bash

    $ cd ~/synfig.git
    
Now you need to install all required dependencies. This is easy to do by running a special script shipped with Synfig's sources:

.. code:: bash

    $ ./1-setup-linux-native.sh

Wait till the script finish installing dependencies and you're ready to build.
    
OSX
-------

We will be running all commands in terminal, so start by launching Terminal app.

First you need to install Xcode Command Line Tools with the following command:

.. code:: bash

    $ xcode-select --install
    
Follow instructions on the screen to complete installation.

Next, get Synfig's sources:

.. code:: bash

    $ git clone https://github.com/synfig/synfig.git ~/synfig.git
    
When download finishes, navigate to sources directory:

.. code:: bash

    $ cd ~/synfig.git
    
Now we can install all required libraries via HomeBrew. There is a special script included with sources:

.. warning::
    It is NOT recommended to use this method on OSX version < 10.11 with already working Homebrew - with almost 100% probablility your Homebrew installation will be damaged. You've been warned.
    
    For more details about this issue see here - https://github.com/synfig/synfig/blob/678cc3a7b1208fcca18c8b54a29a20576c499927/1-setup-osx-brew.sh#L34-L37
    
.. code:: bash

    $ ./1-setup-osx-brew.sh
    
Depending on version of your system the process of installing dependencies might take some time. When it completes you are ready to build Synfig! 

Windows
-------

.. note::
    For compiling Synfig on Windows we use MinGW installation in MSYS2 environment.
    
    Alternative for that approach could be to build using MSVC and Microsoft vcpkg (https://github.com/microsoft/vcpkg), but we haven't digged into that yet. Any help on this matter is appreciated here - https://github.com/synfig/synfig/issues/860.

Download and install MSYS2, following instructions here - http://www.msys2.org/.

After that, start "MSYS2 MinGW 64-bit" from Windows menu and install git:

.. code:: bash

    $ pacman -S git

Next, get Synfig's sources:

.. code:: bash

    $ git clone https://github.com/synfig/synfig.git ~/synfig.git
    
When download finishes, navigate to sources directory:

.. code:: bash

    $ cd ~/synfig.git

Now you need to install all required dependencies. Run a special script shipped with Synfig's sources:

.. code:: bash

    $ ./1-setup-windows-msys2.sh

Wait till the script finish installing dependencies and you're ready to build Synfig.

.. note::
    When Synfig is compiled on Windows using MSYS2 there is a bug related with rendering files via CLI.
    For this reason, generation of GUI icons and images is disabled for Windows build. So, after building
    Synfig Studio you will not see any icons.
    
    For more details about this issue see this bugreport  - https://github.com/synfig/synfig/issues/861
    
First build
~~~~~~~~~~~~~~~~~~~~~~~

We have a special script, which carries all build routines for you. In fact, there are two of them - "2-build-debug.sh" and "2-build-production.sh".

As you might guess, the first one is for building development version with debug symbols (useful for development itself) and the second one is without debug symbols (useful for production).

Another difference is that first script places result of the build in "_debug/build" subdirectory, and with second script the result will reside in "_production/build".

In all other aspects both scripts work exactly the same and accept the same arguments.

I will assume that your intention is to develop Synfig, so let's continue with first script - "2-build-debug.sh".

You can build everything by simply executing the script:

.. code:: bash

    $ ./2-build-debug.sh
    
The script will build and install ETL, then synfig-core and finally - synfig-studio.

When building is done, you can launch Synfig by executing

.. code:: bash

    $ ~/synfig.git/_debug/build/bin/synfigstudio
    
    
Re-building your changes
~~~~~~~~~~~~~~~~~~~~~~~~~~

Of course it is not very efficient to run a full rebuild process on every change. So, the script provides a set of arguments that allow you to execute particular stages of the build:

The syntax is:

.. code:: bash

    $ ./2-build-debug.sh [package] [phase]
    
where

* [package] can have following values:

  * all  - builds all three packages (default).
  * etl - builds ETL only.
  * core - builds synfig-core only.
  * studio - builds synfig-studio only.
  
* [phase] allows you to choose particular phase to execute for given package:

  * clean - does "make clean" operation.
  * configure - running "./configure" script with all neccessary options.
  * make - running "make" command and "make install".
  * build - executes "configure" and "make" phases (default).
  * full - executes all phases: "clean", "configure" and "make" (exactly in that order).

You might ask: why execute those commands/phases from a script , while it is possible to call "./configure" and "make" commands by hand in particular directories? Well, for "make" this would work and is desirable for many cases. But for "./configure" you have to specify many parameters, such as prefix, and locations of some dependent libraries. So it is more convenient to call "./configure" using this helper script.

Examples:

1. Configure and (re)build synfig-core (executes "./configure", "make" and "make install"):

.. code:: bash

    ./2-build-debug.sh core
    
equivalent to:

.. code:: bash

    ./2-build-debug.sh core build

2. Do a full clean build of synfig-core (executes "make clean", "./configure", "make" and "make install"):

.. code:: bash

    ./2-build-debug.sh core full

3. Quick rebuild of synfig-core (without executing "./configure"):

.. code:: bash

    ./2-build-debug.sh core make
    
Since "make" doesn't require any parameters, the same result can be achieved by executing:

.. code:: bash

    cd ~/synfig.git/_debug/synfig-core/
    make install

4. Quick rebuild of of everything - ETL, synfig-core and synfig-studio (without executing "./configure"):

.. code:: bash

    ./2-build-debug.sh all make

Here are some recommendations when to call particular phases:

Considering the structure of Synfig (see first chapter of this article), we have following dependency chain:

**synfig-studio** -> **synfig-core** -> **ETL**

So, you should follow this logic:

* when change is made to **ETL**, then rebuild everything - **ETL**, **synfig-core** and **synfig-studio**;
* when change is made to **synfig-core**, then you need to rebuild **synfig-core** and **synfig-studio** only;
* when change is made to **synfig-studio**, then you have to to rebuild **synfig-studio** only;

You might notice that if you rebuild simply by running "make install" that takes considerably less time than when you do a ful-cycle rebuild with "./configure" and then "make install".

So, when it is safe to skip "./configure"? 

The answer is: if you edited .h and .cpp files only, then it is safe to skip. In all other cases it is safer to re-start ./configure on rebuilding.

Let's suppose you made changes in **synfig-studio** (only .h and .cpp files) and want to rebuild it. The following command is enough:

.. code:: bash

    ./2-build-debug.sh studio make

And finally a quick note about "build.conf.sample" file in the root of source repository.

With this file you can tweak the number of threads used by the build scripts. Just copy "~/synfig.git/build.conf.sample" to "~/synfig.git/build.conf" and adjust its contents according to your needs.

Creating Installer/Package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As an optional step you might wish to build a package/installer for distribution.

First important thing to know is that you need a production build for that (for obvious reason it is very unlikely you want to distribute a build with debug symbols).

So, make sure to get production build first:

.. code:: bash

    ./2-build-production.sh

After build finishes you can generate a package for your operating system.
    
For OSX:

.. code:: bash

    ./3-package-osx-dmg.sh
    
For Linux:

.. code:: bash

    TO BE WRITTEN
    
For Windows:

.. code:: bash

    TO BE WRITTEN
