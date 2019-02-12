.. _building:

Code structure
===============

Things to know before you start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Synfig is divided into three components: ETL, synfig-core and synfig-studio.

* ETL is a template library that implements reference counting, portable threading, gaussian blur, and plenty of other goodies. Every part of the Synfig project uses ETL in some way. It is like the C++ STL.
* synfig-core is Synfig's backend. It renders scenes and knows how to read and write Synfig XML files. This directory contains the Synfig library and the Synfig command-line tool. 
* synfig-studio is the graphical editor. It uses the GTK+ widget library. If you want to hack on the interface, this is what you should look at.

synfig-core
~~~~~~~~~~~~~~~~~~~~~~

The code for synfig-core is located in "synfig-core/src". There you will find 3 important directories:

* "synfig" - a code of "libsynfig" library. It contains code of rendering engine and used by all other Synfig's components. Render engine can be extended with modules (see below).
* "modules" - modules for rendering engine. There are two types of modules:
  * "layers" - allow to add new layer types
  * "targets" - providing support for import/export to various formats.
* "tool" - a code of synfig commandline tool (binary is simply called "synfig"). It uses libsynfig to read Synfig files files and render them in any supported format.

Since with "modules" and "tool" everything pretty much trivial, let's dive into "synfig" - a libsynfig structure.

...

synfig-studio
~~~~~~~~~~~~~~~~~~~~~~

The code for synfig-core is located in "synfig-core/src". There you will find 3 important directories:

* "synfigapp" - ...
* "gui" - ...
* "brushlib" - ...

...
