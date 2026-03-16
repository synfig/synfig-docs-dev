.. _Packaging using CMake:

Packaging using CMake
======================

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

    $ cpack -G DEB

Windows
-------

A *.exe* installer for Synfig can be made by using the following code.

.. code:: bash

    $ cpack -G NSIS

macOS (formely OSX)
-------

A *.app* installer for Synfig can be made by using the following code.

.. code:: bash

    $ cmake --build .

