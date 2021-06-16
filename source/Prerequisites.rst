2. Prerequisites
================

In this chapter, we will talk about the minimum knowledge we need to understand this
report. You will find in this chapter documentation links and tutorials needed to
better start with the rest of the report. Some concepts and skills from other
tutorials are not indicated in the basic documentation from `MRS website <https://ctu-mr
s.github.io>`_ and ROS documentation. That is why we will add more information about
the basic information and minimum requirements on hardware and software sides.

2.1 Hardware minimum requirements
---------------------------------

You need to have a hardware minimum requirements to be able to install and use the
MRS workspace software without any issue. In this section, we will see what kind of
minimum setup requirement is needed, and which configuration is recommended.

    
2.1.1 PC Specs
^^^^^^^^^^^^^^

Before dealing with the Ubuntu partition, we have to talk about your minimum PC
configuration. Here you can see the general PC specs used by the four interns during
the project:

.. figure:: Intern_PC_specs.png
   :width: 800
   :align: center

   Figure 2.1: Intern PC specs

However, this configuration above could not be enough when we have to launch simulations
that need a lot of resources from our PC. We may have issues with the spawn and takeoff
of the UAV later. That is why we rather recommend this kind of PC specs:

.. figure:: Bryan_PC_specs.png
   :width: 800
   :align: center

   Figure 2.2: Bryan Convens PC specs

This type of configuration is better for simulation launch. We rather recommend a PC with
a processor in this category. Indeed, the CPU performance will have a significant impact
on the build of the packages in your workspace, and the proper launch of your simulation
when you will need to launch more than three UAV.

Note: Keep in mind that some issues with your simulation will not be related to your PC
configuration. We had issues with the launch of many UAV, due to the lack of code
optimization.

2.1.2 Ubuntu partition
^^^^^^^^^^^^^^^^^^^^^^

2.1.3 Prepare Ubuntu
^^^^^^^^^^^^^^^^^^^^

2.2 MRS workspace installation
------------------------------

2.3 Working with ROS
--------------------

2.4 Working with Tmux session
-----------------------------

2.5 Working with Git
--------------------

2.6 How to use github permalinks
--------------------------------