.. _Building using CMake:

Building Synfig Using CMake
============================

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

.. note::
   Before commit `3f07959 <https://github.com/synfig/synfig/commit/3f0795986a0e0612f86695dade1ebd4f658d3c39>`_, the special script
   was named `./2-build-msys-cmake.sh` instead of `./2-build-cmake.sh`.
   Same applies to the default build directory: it was named ./cmake-build-msys back then.

.. code:: bash

    $ ./2-build-cmake.sh

Once this is completed successfully, you can run Synfig by.

.. code:: bash

    $ ./cmake-build/output/Release/bin/synfigstudio.exe

You can also use this script to run a debug build by using the argument: Debug

.. code:: bash

    $ ./2-build-cmake.sh Debug
    
Then, you can run Synfig by.

.. code:: bash

    $ ./cmake-build/output/Debug/bin/synfigstudio.exe
