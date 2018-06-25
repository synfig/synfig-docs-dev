Random notes
===============

Building Synfig for production
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Base dependency to run Synfig on Linux systems is **glibc**. It has backward compatibility,
but not forward. So if you build Synfig on newer OS like Debian 9 it will not start on older
OS like Debian 7. To do that we use Debian 7 docker image for building Synfig for production.
For testing/debug purposes or for own usage you can build Synfig on any preferred OS.
glibc versions (obtained by using ldd --version):
* Debian 7.11 - 2.13
* Debian 8.10 - 2.19

How to login to docker image?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo docker run -it debian/wheezy-backports /bin/bash -l

