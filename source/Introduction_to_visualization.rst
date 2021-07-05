5. Introduction to visualization
================================

.. role:: raw-html(raw)
    :format: html

In this chapter, we will introduce you to the visualization on Rviz. It is a great tool for ROS, used by many to debug codes or to have some nice
visualization of your simulations.

5.1 The beginning
-----------------

First, especially if you have never runned any simulation using Rviz, we recommand you to try some CTU MRS simulation.
To do that you can run these commands: ::

    cd ~/mrs_workspace/src/simulation/example_tmux_scripts/one_drone_gps
    ./start.sh

The Rviz window will open after Gazebo and it looks like this:

.. figure:: _static/one_drone_rviz.png
   :width: 800
   :align: center

   Figure 5.1: RViz window

You can see arrows, poses and UAV frames in this simple visualization.
For these simple tasks, you only need to click on the add button and subscribe to the topic you want to visualize.

.. figure:: _static/add_button.png
   :width: 500
   :align: center

   Figure 5.2: Add button

.. figure:: _static/topic_window.png
   :width: 800
   :align: center

   Figure 5.3: Topic window

You can also use the 2D Nav Goal button to choose a position and a heading to go for the UAV.

.. figure:: _static/navgoal_button.png
   :width: 500
   :align: center

   Figure 5.4: Navigation goal button

Next, you can run some simulations which use dedicated plugins for one specific task.
You will need to use these commands: ::

    cd ~/mrs_workspace/src/uav_core/ros_packages/mrs_uav_testing/tmux
    ls

It will show you the different simulation that you can test. You can do it by using ``cd ./"directory_of_the_simulation"`` and running: ::

    ./start.sh

The bumper simulation is an example of advanced task that you can do on Rviz. It is made by a plugin created from scratch.
It represents a huge work to create these type of visualization but it shows you the diversity of possibilities.

5.2 How RViz works ?
--------------------

To run a simulation, you will use the ``start.sh`` file wich will ask to the ``session.yml`` what ``.launch`` file are going to be runned. 
A ``.yml`` looks like this:

.. figure:: _static/yml_file.png
   :width: 800
   :align: center

   Figure 5.5: .yml file

You can see that there is an Rviz part. 
The first line ask for the ``rviz.launch`` file which is used to choose the ``.rviz`` file that you want to use. This type of file is used to save
the configuration of Rviz, like what is displayed. On the ``testing_brubotics`` package, there is the ``rviz`` directory which contains all the ``.rviz``
files.

You can generate a ``.rviz`` file, which save your RViz configuration, by clicking in RViz on ``File->Save config as``.

5.3 D-ERG strategies
--------------------

We want to visualize what it is computed by the D-ERG tracker of BruBotics, especially in the `2_two_drones_D-ERG simulation <https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/bryan/2_two_drones_D-ERG>`__ 
that you can run with these commands: ::

    cd ~workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/2_two_drones_D-ERG/
    ./start.sh

We have several D-ERG (Distributed Explicit Reference Governor) strategies to illustrate. For more advanced explanations, watch `this video <https://www.youtube.com/watch?v=le6WSeyTXNU>`__

5.3.1 D-ERG strategy 0
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-0.png
   :width: 500
   :align: center

   Figure 5.6: D-ERG strategy 0

* p\ :sub:`k`\: current pose of the UAV
* p̂\ :sub:`k`\: desired reference pose
* p\ :sub:`k`\ :sup:`v`\: applied reference pose 
* R\ :sub:`a`\: drone's radius

Communicate: p\ :sub:`k`\

Sphere can **translate**.

:raw-html:`<font color="RoyalBlue">[Explanations of how we did it.]JV</font>`

5.3.2 D-ERG strategy 1
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-1.png
   :width: 500
   :align: center

   Figure 5.7: D-ERG strategy 1

Communicate: p\ :sub:`k`\, p\ :sub:`k`\ :sup:`v`

Tube can **translate** and **rotate**.

5.3.3 D-ERG strategy 2
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-2.png
   :width: 500
   :align: center

   Figure 5.8: D-ERG strategy 2

Communicate: p\ :sub:`k`\, p\ :sub:`k`\ :sup:`v`

Tube can **translate**, **rotate** and **change length**.

5.3.4 D-ERG strategy 3
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-3.png
   :width: 500
   :align: center

   Figure 5.9: D-ERG strategy 3

Communicate: p\ :sub:`k`\, p\ :sub:`k`\ :sup:`v`, S\ :sub:`a,min`\ :sup:`⊥`

Tube can **translate**, **rotate**, **change length and width**. The witfh (radius) is the minimal one for a tube with error directed longitudinal axis.

5.3.5 D-ERG strategy 4
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-4.png
   :width: 500
   :align: center

   Figure 5.10: D-ERG strategy 4

Communicate: p\ :sub:`k`\ :sup:`0`, p\ :sub:`k`\ :sup:`1`, S\ :sub:`a,min`\ :sup:`⊥`

Tube and cylinder can **translate**, **rotate**, **change length and width**. The width (radius) and the length are the minimal one for a tube with error directed
longitudinal axis.

5.3.6 D-ERG strategy 5
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-5.png
   :width: 500
   :align: center

   Figure 5.11: D-ERG strategy 5

This final strategy permits to calculate the minimal distance between 2 drones.

5.4 Our work
------------

:raw-html:`<font color="RoyalBlue">[Explanations of how we did it.]JV</font>`
