5. Introduction to visualization
================================

In this chapter, we will introduce you to the visualization on Rviz. It is a great tool for ROS, used by many to debug codes or to have some nice
visualization of your simulations.

5.1 The beginning
-----------------

First, especially if you have never runned any simulation using Rviz, we recommand you to try some CTU MRS simulation.
To do that you can run these commands:

.. code-block:: shell

   cd ~/mrs_workspace/src/simulation/example_tmux_scripts/one_drone_gps
   ./start.sh

The RViz window will open after Gazebo and it looks like this:

.. figure:: _static/one_drone_rviz.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 5.1: RViz window with CTU visualization

You can see arrows, poses and UAV frames in this simple visualization.
For these simple tasks, you only need to click on the add button and subscribe to the topic you want to visualize.

.. figure:: _static/add_button.png
   :width: 400
   :alt: alternate text
   :align: center

   Figure 5.2: Add button

.. figure:: _static/topic_window.png
   :width: 400
   :alt: alternate text
   :align: center

   Figure 5.3: Topic window

You can also use the 2D Nav Goal button to choose a position and a heading to go for the UAV.

.. figure:: _static/navgoal_button.png
   :width: 400
   :alt: alternate text
   :align: center

   Figure 5.4: Navigation goal button

Next, you can run some simulations which use dedicated plugins for one specific task.
You will need to use these commands:

.. code-block:: shell

   cd ~/mrs_workspace/src/uav_core/ros_packages/mrs_uav_testing/tmux
   ls

It will show you the different simulation that you can test. You can do it by using ``cd ./"directory_of_the_simulation"`` and running:

.. code-block:: shell

   ./start.sh

The bumper simulation is an example of advanced task that you can do on Rviz. It is made by a plugin created from scratch.
It represents a huge work to create these type of visualization but it shows you the diversity of possibilities.

5.2 How RViz works ?
--------------------

To run a simulation, you will use the ``start.sh`` file wich will ask to the ``session.yml`` what ``.launch`` file are going to be runned. 
A ``.yml`` looks like this:

.. figure:: _static/yml_file.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 5.5: .yml file

You can see that there is an Rviz part. 
The first line ask for the ``rviz.launch`` file which is used to choose the ``.rviz`` file that you want to use. This type of file is used to save
the configuration of Rviz, like what is displayed. On the ``testing_brubotics`` package, there is the ``rviz`` directory which contains all the ``.rviz``
files.

You can generate a ``.rviz`` file, which save your RViz configuration, by clicking in RViz on ``File->Save config as``.

5.3 Structure of the visualization_brubotics package
----------------------------------------------------

We have developed a `visualization package <https://github.com/mrs-brubotics/visualization_brubotics>`__ which permits to visualize on RViz the previous
strategies in the `2_two_drones_D-ERG simulation <https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/bryan/2_two_drones_D-ERG>`__.
This package is based on the `mrs_rviz_plugins <https://github.com/ctu-mrs/mrs_rviz_plugins>`__ structure. We will explain you how to reproduce it.

First, create a new package in ``workspace/src_droneswarm_brubotics/ros_packages`` with:

.. code-block:: shell

   catkin_create_pkg visualization_brubotics

This command creates a ``CMakeLists.txt`` file and a ``package.xml`` file.

Then, go to the ``session.yml`` file of the `2_two_drones_D-ERG simulation <https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/bryan/2_two_drones_D-ERG>`__.
At the end (line 223), you should see a RViz part. If it is commented, uncomment it. Modify these lines so it looks lite this:

.. code-block:: shell

   - rviz:
      layout: tiled
      panes:
        - waitForControl; roslaunch visualization_brubotics rviz.launch name:=avoidance_test
        - waitForControl; roslaunch visualization_brubotics load_robot.launch
  
5.3.1 `launch folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/launch>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now, create a ``launch`` folder in your ``visualization_brubotics`` package and copy/paste the ``mrs_uav_testing/launch/rviz.launch`` file.
Open it and change ``mrs_uav_testing`` by ``visualization_brubotics``. This file permits to open a RViz window when you will start the `2_two_drones_D-ERG simulation <https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/bryan/2_two_drones_D-ERG>`__.

Copy/paste the ``mrs_uav_testing/launch/tf_connector_avoidance.launch`` file in your ``launch`` folder and rename it ``load_robot.launch``. Open it and make the
following changes:

.. code-block:: xml

   <launch>

     <arg name="uav_type" default="$(optenv UAV_TYPE f450)"/>

         <!-- other args -->
     <arg name="standalone" default="true" />
     <arg name="debug" default="false" />

     <arg     if="$(eval arg('standalone') or arg('debug'))" name="nodelet" value="standalone" />
     <arg unless="$(eval arg('standalone') or arg('debug'))" name="nodelet" value="load" />
     <arg     if="$(eval arg('standalone') or arg('debug'))" name="nodelet_manager" value="" />
     <arg unless="$(eval arg('standalone') or arg('debug'))" name="nodelet_manager" value="tf_connector_nodelet_manager" />

     <arg     if="$(arg debug)" name="launch_prefix" value="debug_roslaunch" />
     <arg unless="$(arg debug)" name="launch_prefix" value="" />

     <group ns="uav1">
       <param name="robot_model" command="$(find visualization_brubotics)/scripts/generate_robot_model_xml.py $(find visualization_brubotics)/data/$(arg uav_type).xml uav1/fcu $(find visualization_brubotics)" />
       <node name="tf_published_uav_marker_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav1/fcu uav1/fcu/uav_marker" />
       <node name="tf_published_props_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav1/fcu uav1/fcu/props" />
       <node name="tf_published_arms_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav1/fcu uav1/fcu/arms" />
       <node name="tf_published_arms_red_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav1/fcu uav1/fcu/arms_red" />
     </group>

     <group ns="uav2">
       <param name="robot_model" command="$(find visualization_brubotics)/scripts/generate_robot_model_xml.py $(find visualization_brubotics)/data/$(arg uav_type).xml uav2/fcu $(find visualization_brubotics)" />
       <node name="tf_published_uav_marker_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav2/fcu uav2/fcu/uav_marker" />
       <node name="tf_published_props_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav2/fcu uav2/fcu/props" />
       <node name="tf_published_arms_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav2/fcu uav2/fcu/arms" />
       <node name="tf_published_arms_red_link" pkg="tf2_ros" type="static_transform_publisher" args="0 0 0 0 0 0 uav2/fcu uav2/fcu/arms_red" />
     </group>

     <node pkg="nodelet" type="nodelet" name="tf_connector_dummy" args="$(arg nodelet) mrs_uav_odometry/TFConnectorDummy $(arg nodelet_manager)" output="screen" launch-prefix="$(arg launch_prefix)">

       <rosparam file="$(find visualization_brubotics)/config/tf_connector_avoidance.yaml" />

       <!-- Subscribers -->
       <remap from="~tf_in" to="/tf" />

       <!-- Publishers -->
       <remap from="~tf_out" to="/tf" />

     </node>

   </launch>


This file will launch the 2 UAV on the RViz window.

Go in the ``launch`` folder from ``mrs_rviz_plugins``, copy the ``rviz_interface`` folder and paste it in your ``visualization_brubotics/launch``
folder. You will be able to use tools developed by CTU like the "2D Nav Goal" after you did :ref:`these steps <5.4.6_src_folder>`.

5.3.2 `rviz folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/rviz>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create a ``rviz`` folder in your ``visualization_brubotics`` package. Copy/paste the ``mrs_uav_testing/rviz/default_simulation.rviz`` in it. 
Create a ``avoidance_test.rviz file``, copy the text from `our existing file <https://github.com/mrs-brubotics/visualization_brubotics/blob/main/rviz/avoidance_test.rviz>`__
and paste it in the file you just have created. It will allow you to directly see on RViz what is interesting to visualize.

5.3.3 `data folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/data>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For this step, you only have to copy/paste the entire ``mrs_rviz_plugins/data`` folder in your ``visualization_brubotics`` package. This folder contains the
description of the UAV models.

5.3.4 `scripts folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/scripts>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create a ``scripts`` folder in your ``visualization_brubotics`` package and copy/paste the ``mrs_rviz_plugins/scripts/generate_robot_model_xml.py`` file in it.
This script will generate a ``robot_model`` that you will be able to visualize on RViz.

5.3.5 `config folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/config>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create a ``config`` folder in your ``visualization_brubotics`` package and copy/paste the ``mrs_uav_testing/config/tf_connector_avoidance.yaml`` in it.
This file will define properly the frames id.

.. _5.4.6_src_folder:

5.3.6 `src folder <https://github.com/mrs-brubotics/visualization_brubotics/tree/main/src>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Finally, create a ``src`` folder in your ``visualization_brubotics`` package and copy/paste the ``mrs_rviz_plugins/src/rviz_interface`` folder in it.
It contains 2 ``.cpp`` files which define who the CTU RViz tools work.

5.4 Our work: D-ERG visualization
---------------------------------

We want to visualize what it is computed by the `D-ERG tracker <https://github.com/mrs-brubotics/trackers_brubotics/blob/master/src/dergbryan_tracker/dergbryan_tracker.cpp>`__ of
BruBotics, especially in the `2_two_drones_D-ERG simulation <https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/bryan/2_two_drones_D-ERG>`__ that you can
run with these commands:

.. code-block:: shell

    cd ~workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/2_two_drones_D-ERG/
    ./start.sh

We have several D-ERG (Distributed Explicit Reference Governor) strategies to illustrate. For more advanced explanations, watch `this video <https://www.youtube.com/watch?v=le6WSeyTXNU>`__

5.4.1 D-ERG strategy 0
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-0.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.6: D-ERG strategy 0

* :math:`p_{k}`: current pose of the UAV
* :math:`p̂_{k}`: desired reference pose
* :math:`p_{k}^{v}`: applied reference pose 
* :math:`R_{a}`: drone's radius

Communicate: :math:`p_{k}`

Sphere can **translate**.

In order to visualize how it works, we first need to change ``data/f450.xml`` file. The error sphere has a constant radius so it is easy: you just need to add a marker like this:

.. code-block:: xml

   <link name="[REPLACEME]uav_name[/REPLACEME]/uav_marker">
     <!-- UAV specific-color marker -->
     <visual>
       <origin xyz="0 0 -70e-3" rpy="0 0 0" />
       <geometry>
         <cylinder radius="370e-3" length="220e-3" />
       </geometry>
       <material name="UAVSpecificColor" />
     </visual>
     <visual>
       <origin xyz="0 0 -70e-3" rpy="0 0 0" />
       <geometry>
         <sphere radius="1.5" />
       </geometry>
       <material name="UAVSpecificColor" />
     </visual>

:blue:`[TODO: explanations about how to visualize the path, the applied pose and desired reference pose]JV`

5.4.2 D-ERG strategy 1
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-1.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.7: D-ERG strategy 1

Communicate: :math:`p_{k}`, :math:`p_{k}^{v}`

Tube can **translate** and **rotate**.

To visualize a pill, we need to create a plugin because this display type is not available on RViz. But this is not trivial at all.

:blue:`[TODO: explanations about how to do it]JV`

5.4.3 D-ERG strategy 2
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-2.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.8: D-ERG strategy 2

Communicate: :math:`p_{k}`, :math:`p_{k}^{v}`

Tube can **translate**, **rotate** and **change length**.

:blue:`[TODO: explanations about how to do it]JV`

5.4.4 D-ERG strategy 3
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-3.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.9: D-ERG strategy 3

Communicate: :math:`p_{k}`, :math:`p_{k}^{v}`, :math:`S_{a,min}^{⊥}`

Tube can **translate**, **rotate**, **change length and width**. The witfh (radius) is the minimal one for a tube with error directed longitudinal axis.

:blue:`[TODO: explanations about how to do it]JV`

5.4.5 D-ERG strategy 4
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-4.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.10: D-ERG strategy 4

Communicate: :math:`p_{k}^{0}`, :math:`p_{k}^{1}`, :math:`S_{a,min}^{⊥}`

Tube and cylinder can **translate**, **rotate**, **change length and width**. The width (radius) and the length are the minimal one for a tube with error directed
longitudinal axis.

:blue:`[TODO: explanations about how to do it]JV`

5.4.6 D-ERG strategy 5
^^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/DERG-5.png
   :width: 500
   :alt: alternate text
   :align: center

   Figure 5.11: D-ERG strategy 5

This final strategy permits to calculate the minimal distance between 2 drones.

:blue:`[TODO: explanations about how to do it]JV`
