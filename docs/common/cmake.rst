.. _cmake:

Synfig Using CMake
==================

Synfig is written in C++, based on GTK3 library. This documentation uses CMake for building. Synfig can also be built using aututools/make. Check that out on :ref:`this page <building>`.

Build system: CMake


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

Make sure to select the correct MSYS as per your needs. 

.. image:: ../images/msys.png

Always use the proper shell:

    * **MinGW32** for compiling **32**-bit Synfig.
    * **MinGW64** for compiling **64**-bit Synfig.
    * **Never** use the **MSYS** shell for compiling Synfig.

After picking the needed MSYS Shell.

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

Wait till the script finish installing dependencies. Now install CMake and Ninja with:

    * For MINGW32:
    .. code:: bash

        $ pacman -S mingw-w64-i686-cmake mingw-w64-i686-ninja

    * For MINGW64:
    .. code:: bash

        $ pacman -S mingw-w64-x86_64-cmake mingw-w64-x86_64-ninja

When it completes you are ready to build Synfig! 

    
Building using CMake
~~~~~~~~~~~~~~~~~~~~~~~

The usual process of building with CMake is pretty much the same as the standard one, but despite that, this part will cover that so you can follow along and get the same results.

Below you will find instructions how to build Synfig on various operating systems.

Linux
-----

You need to make a directory where Synfig will be built.

.. code:: bash
    
    $ mkdir cmake-build && cd cmake-build

Once you're in your build folder, this is the part where you can configure and set different CMake flags according to your need. For the purpose of this documentation, the default build will be followed.

.. code:: bash

    $ cmake ..

When it completes, you need to build Synfig using *make* command. 

.. code:: bash

    $ make

Once this is complete, you will find the build present in `/output` folder of your build directory. You can run Synfig by using

.. code:: bash

    $ ./output/bin/synfigstudio

You can also specify a target if you want to build that specific target.

.. code:: bash

    $ make build_images

If you wish to install Synfig to `/usr/local/`, make sure you have root privileges (or write permissions to those three directories), and simply type

.. code:: bash

    $ make install

OSX
---

Building on OSX follows the same steps as that of Linux. You need to make a build folder first.

.. code:: bash

    $ mkdir cmake-build && cd cmake-build

Once that's done, you can run the CMake command along with passing any flags as required.

.. code:: bash

    $ cmake ..

When it completes, you need to build Synfig using *make* command. 

.. code:: bash

    $ make

Once this is complete, you will find the build present in `/output` folder of your build directory. You can run Synfig by using

.. code:: bash

    $ ./output/bin/synfigstudio

Windows
-------

Synfig is shipped with this special script to build it on Windows (MSYS2). All you need to do is run that script.

.. code:: bash

    $ ./2-build-msys-cmake.sh

Once this is completed successfully, you can run Synfig by.

.. code:: bash

    $ ./cmake-build-msys/output/bin/synfigstudio.exe

Packaging using CMake
~~~~~~~~~~~~~~~~~~~~~

As an optional step you might wish to build a package/installer for distribution.

Synfig can be packaged for different operating systems using CPack which comes with CMake. 

First important thing to know is that you need a production build for that (for obvious reason it is very unlikely you want to distribute a build with debug symbols).

So, make sure to get production build first by passing the flag at *cmake* step:

.. code:: bash

    $ cmake .. -D CMAKE_BUILD_TYPE=Release

Most of the configuration are already set so no extra flags need to be set for Synfig. 

Below you will find instructions how to package Synfig for various operating systems.

Linux
-----

A *.deb* package for Synfig can be made by using the following code.

.. code:: bash

    $ make package

OSX
---

TO BE WRITTEN

Windows
-------

A *.exe* installer for synfig can be made by using the following code.

.. code:: bash

    $ make package

