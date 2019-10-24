Setting up Netbeans IDE for Synfig Development
==============================================

If you are a developer yourself, you might want to setup a development environment, so that you can debug and develop Synfig Studio yourself.
This tutorial will guide you on setting up Apache Netbeans IDE for Synfig.
It is recommended that you clone the Synfig project before following this guide :)

Prerequisites
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Please ensure that you have the following prerequisites installed on your machine:

    * Git
    * Oracle’s Java 8 or Open JDK v8

.. note::

     **TL;DR**
     You can also refer to this video tutorial -> https://youtu.be/op-PShrJTCM


Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Apache Netbeans IDE is recommended for Synfig Studio development. The Installer automatically prepares the IDE so that it comes with all required plug-ins, the correct workspace encoding settings, pre-configured code formatters and more. Simply follow these steps:

    * Download the `Netbeans Installer. <https://netbeans.apache.org/download/>`_
    * Launch the Netbeans IDE and go to tools -> plugins.
    * Go to Settings tab and enable **Netbeans 8.2 plugins portal**:
        
        .. image:: ../images/netbeans_plugin_settings.png

    * Go to Available plugins and click on **check for newest**.
    * Now select and install C/C++ plugin:
        
        .. image:: ../images/netbeans_cpp_plugin.png


Setup New Synfig Project
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
IDE Installation is now completed so let's configure our new synfig project.

    * Click file -> new project 
    * Select C/C++ and C/C++ Project with Existing Source:

        .. image:: ../images/netbeans_choose_project.png

    * Select your synfig source file directory
    * Under **Select configuration mode** select **custom** and press next:
        
        .. image:: ../images/netbeans_select_mode.png

    * Click next and skip the Pre Build Action(do not check any option).
    * Under Build Action uncheck the **clean and build after finish** and click next:
        
        .. image:: ../images/netbeans_build_actions.png

    * Now click next on step 5 and step 6 without making any changes.
    * Click finish:
        
        .. image:: ../images/netbeans_setup_finish.png


Setting up make configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Now we will configure make settings for our synfig project.

    * Right click on project(Synfig) and select properties:
        
        .. image:: ../images/netbeans_make_1.png

    * Under Build -> make section change Build result so that it points to "_debug/build/bin/synfigstudio":
        
        .. image:: ../images/netbeans_make_2.png

    * Press apply and ok.


**Yay! your Netbeans IDE is now configured and you can use all the functionalities including debugging etc.**