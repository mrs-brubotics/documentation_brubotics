Project Certified Correct Control of UAVs with Cable-Suspended-Payload
=========================================================================

.. admonition:: todo

   Raphael todo


Introduction
---------------


Simulation setup
----------------------------------------------------

.. admonition:: todo

   Raphael todo: explain how the changes in the software allow a Gazebo simulation which can simulate cable suspended payload UAVs (1 or 2), 
   how the data of Gazebo (which topics etc) is going into the controller and tracker 
   and which tests can be ran with this new feature. These are the tests in your thesis. 
   So only required to put the basic validations in here that proof your changes are correct.


In this section, you will learn how to:

* make a load spawn in Gazebo;
* modify the load model;
* visualize it using Rviz;
* tweak the simulation parameters according to your needs. 

Creation of a new world with payload and automate it to start with simulation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a new simulation environment, follow these steps:

* Make a new folder in the directory: */workspace/src/droneswarm\_brubotics/ros\_packages/testing\_brubotics/tmux\_scripts*.

* Copy the files ("start.sh", "layout.json" and "session.yml") of an other example (from brubotics or mrs) in your new folder. Take a simulation as close as possible to 
  what you want to achieve (e.g.:one drone gps). 

* Launch your simulation, by opening terminal and typing "./start.sh". 

* If you want to create a new world, copy an existing world file in in the folder 
  you created in *.../tmux\_scripts* . The location of the standard mrs world files are in following directory: */git/simulation/ros\_packages/mrs\_gazebo\_common\_resources*. For our situation, we started from the standard grass plane. 

* Adapt your file by adding models, e.g. boxes or trees. We added a box that will be used as payload. Find exemples of codes to implement this models in other .world files on 
  the internet, or follow the example presented in next section. 

* Change "world\_name:=grass\_plane" in the following lines in the "Session.yml" file:

.. code-block:: xml

  - gazebo:
    layout: tiled
    panes:
    - waitForRos; roslaunch mrs_simulation simulation.launch world_name:=grass_plane gui:=true

with these lines :
  
.. code-block:: xml

  - gazebo:
    layout: tiled
    panes:
      - waitForRos; roslaunch mrs_simulation simulation.launch world_file:='/home/NAME OF COMPUTER SESSION/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/NAME OF FOLDER/NAME OF WORLD FILE.world' gui:=true



Link_attacher
^^^^^^^^^^^^^^^

Installation
****************************
The required package is normally installed directly with the dronesware brubotics installation. Check in *~/workspace/src/* if mrs_gazebo_extras_resources is present or not.
If it's there, skip the first step (cloning) and only do the next step (.worlf file modification).

To be able to attach a load to your drone, follow next steps:

* Exectute the following commands to clone the extra recources file of MRS:

.. code-block:: shell

  cd ~/workspace/src/
  git clone https://github.com/ctu-mrs/mrs_gazebo_extras_resources

* Build the workspace:

.. code-block:: shell

  catkin build

* Add the plugin of the link_attacher in the .world file:

.. code-block:: xml

  <plugin name="mrs_gazebo_link_attacher_plugin" filename="libMRSGazeboLinkAttacherPlugin.so"/>

Creation of a link
**********************

Now you can use the link attacher plugin in your
simulation. To be able to use the plugin, there must be an object in your .world file to attach to your
drone (a box for example):

1. Start your simulation from previous chapter.

2. To create a box,create a new file named "box.urdf" and copy paste the following code inside (or put it inside the .world file.):

.. code-block:: xml 

  <?xml version="1.0" ?>
  <robot name="box" xmlns:xacro="http://www.ros.org/wiki/xacro">
          <!-- 1st link -->
      <link name="link_chassis">
          <pose>0 0 0 0 0 0</pose>
          <inertial>
              <mass value="0.5"/>
              <origin xyz="0 0 0.1" rpy="0 0 0"/>
              <inertia ixx="0.0395" ixy="0" ixz="0" iyy="0.106" iyz="0" izz="0.1062"/>
          </inertial>
          <collision name="collision_chassis">
              <geometry>
                  <box size=".5 .5 .5"/>
              </geometry> 
          </collision>
          <visual>
          <origin rpy="0 0 0" xyz="0 0 0"/>
              <geometry>
                  <box size=".5 .5 .5"/>
              </geometry>
          </visual>
      </link>
  </robot>

Then create another file called "box.launch" and copy paste the following code inside (if you've chosen to extend the .world file, you can skip this part):

.. code-block:: xml 

  <?xml version="1.0"?>
  <launch>
      <param name="robot_description" command="$(find xacro)/xacro '$(find testing_brubotics)/tmux_scripts/PATH/box.urdf'" />
      <arg name="x" default="0"/>
      <arg name="y" default="0"/>
      <arg name="z" default="1.5"/>

      <node name="SpawnBox" pkg="gazebo_ros" type="spawn_model" output="screen" args="-urdf -param robot_description -model load -x $(arg x) -y $(arg y) -z $(arg z)" />
  </launch>

Don't forget to change the path leading to the URDF file. The content of these two files will be explained in the next chapter.
To make the box spawn, after a simulation has been started, open a new shell and paste this:

.. code-block:: shell

  roslaunch testing_brubotics box.launch

Then move your drone above the object you want to connect it with.
The distance between the drone and the object will be the length of the link. 

3. Create the link by performing following commands in a new shell tab, while adapting all the names
between parentheses to your situation. The correct model and link names can be seen in gazebo.

.. code-block:: shell

  rosservice call /link_attacher_node/attach "model_name_1: 'uav1'
  link_name_1: 'base_link'
  model_name_2: 'unit_box'
  link_name_2: 'link1' "

This link will create a distance constraint, between the links of the two models. This means the
objects will always stay at a same distance from each other. The link will however not be visible.
The links are placed in the center of mass of a standard object. We will later, in section 5.4, see
how links can be placed at other places than the center of mass.

4. If the connection succeeded, the message "ok: True" will be given. It could not succeed if you wrote
the names of your links and models wrong. 

5. You can also change the joint type by adding "joint_type: ’INSERT_TYPE’" as shown below. The possible choices
are "revolute", "ball", "gearbox", "prismatic", "revolute2", "universal", "piston", "fixed". If you do
not specify the joint type, it will be a revolute joint. The joint type you define will be the joint
connecting the first model with the link, the connection of the second model to the link, will be
fixed.

.. code-block:: shell

  rosservice call /link_attacher_node/attach_typed "model_name_1: 'uav1'
  link_name_1: 'base_link' model_name_2: 'unit_box' link_name_2: 'link1'
  joint_type: 'ball'"

In our situation we want a ball joint (spherical joint), to approach a cable on a hinge

6. Now you can move your drone up to see your payload take off. Try moving your drone sideways,
you will see the payload is not implemented yet in the controller and there will be oscillations.

Here is what you should see in your simulation :

.. figure:: _static/Link_attacher.png
   :width: 800
   :alt: alternate text
   :align: center


Model your payload with an URDF file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Instead of spawning the box in the world file as done previously, it is possible to make an urdf file of the
payload. This has the advantage that you can define more comlex connections of multiple objects and
add joints between elements.

Create urdf file
*************************

Open a blank file and save it as MODELNAME.urdf, for the MODELNAME
you can choose what you want. Place the urdf file in an existing package or make a new package. To reproduce the steps and learn correctly, 
create a new folder in testing_brubotics/load_transportation. 
In the following code we have an example to model a box. You can copy and paste this code in the blank urdf file, then save the document. 

.. code-block:: xml

  <?xml version="1.0" ?>
  <robot name="ROBOTNAME" xmlns:xacro="http://www.ros.org/wiki/xacro">
  
     <!-- 1st link -->
    <link name="link_chassis">
      <pose>0 0 0.1 0 0 0</pose>
      <inertial>
        <mass value="5"/>
        <origin xyz="0 0 0.1" rpy="0 0 0"/>
        <inertia ixx="0.0395" ixy="0" ixz="0" iyy="0.106" iyz="0" izz="0.1062"/>
      </inertial>
      
      <collision name="collision_chassis">
        <geometry>
          <box size="1 1 2"/>
        </geometry>
        </collision>
      <visual>
        <origin rpy="0 0 0" xyz="0 0 0"/>
        <geometry>
          <box size="1 1 2"/>
        </geometry>
      </visual>
    </link>
  </robot>

The <xml> line is a standard line then in the second line of code you have to give a name to your robot
(ROBOTNAME), you can change what you want for example "payload". Start the robot description with
<robot>. The next step is to make the links and joints. There are some sub modules like inertial, collision
and visual. Again you can name them how you want. The sub modules can be modified and the collision
and visual do not have to be the same. More info can be found on http://wiki.ros.org/urdf/XML/link.
Finally, close the robot description with </robot>.

Create a launch file
**********************

Now that you have created the urdf file, it needs to be executed. Therefore we use a launch file. Again
open a blank file and save it as NAME.launch, with "NAME" that can be what you want. Place
it in the folder with all the other documents you created in testing_brubotics/load_transportation. Below an example of a launch file 
is shown, you can copy paste this code inyour launch file.

.. code-block:: xml

  <launch>
    <param name="robot_description" command="$(find xacro)/xacro '$(find testing_brubotics)/tmux_scripts/FOLDERNAME/MODELNAME.urdf'" />
    
    <arg name="x" default="0"/>
    <arg name="y" default="0"/>
    <arg name="z" default="1.5"/>
    
    <node name="NODENAME" pkg="gazebo_ros" type="spawn_model" output="screen"
          args="-urdf -param robot_description -model ROBOTNAME -x $(arg x) -y $(arg y) -z $(arg z)" />
          
  </launch>

Again, the first line of code is as standard line that has to be put. Start the launch file with <launch>
on the second line. The param name="robot_description" is a package in ROS and cannot be changed.
Then the command find xacro is executed, this tries to find the urdf file in the path you provide. Change
the correct names that are in UPPERCASE to your directory and urdf file name!
Then some arg are defined, "x, y and z", this is were the urdf file will be spawned. You can change
those values. Finally, you create a node with "NODENAME" that can be changed to what you want for
example, spawn_payload. The pkg used is gazebo_ros with a certain type and the result is shown on the
screen. The arguments are given to the urdf file where you need to change the ROBOTNAME, to the
name you gave in the urdf file!
To test if everything works as expected launch a simulation (./start.sh in the right folder). Then
execute the launch file by opening a new terminal and pasting the following command (change the name
to your NAME.launch file).

.. code-block:: shell

  roslaunch testing_brubotics NAME.launch


 You should see a box spawn like on the following figure::

.. figure:: _static/urdf_install.png
  :width: 800
  :alt: alternate text
  :align: center

Automate this using tmux
***************************

Instead of opening a new terminal it is possible to do it with the rest
of the simulation. Open for that your session.yml file in your directory. Add the lines that are indicated
below between the spawn and control code, and change the NAME.launch to your actual launch file. Save
then exit the document. Now when executing ./start.sh you should see the box spawn in your world. The
lines added will execute the launch file.

.. code-block:: xml

  - load:
      layout: tiled
      panes:
        - waitForSimulation; roslaunch testing_brubotics NAME.launch

Model your payload with an XACRO file
**************************************

The advantage with using xacro files is that we can use macros. This means that instead of defining each
link in the urdf file we can make a macro. A macro acts line a function were we give variables and this
makes a link. This means that we use 2 xacro files, one where the "functions" are defined and one were
the parameters are given. Because the number of files begins to increase, sub folders are made to have a
clearer overview like on the figure below. Later the files will be put in the right folder and pushed to the
brubotics github.

.. figure:: _static/structure.png
   :width: 800
   :alt: alternate text
   :align: center

.. note::
  For a more complete introduction, follow `this youtube tutorial <https://www.youtube.com/watch?v=ixTMFQfXfgs>`__ (part 1 to 4 are relevant to learn URDF,XACRO and using Rviz efficiently).

The first step you need to do is make a xacro file. This is done by opening a blank file and saving it
as MODELNAME.xacro. In your launch file change the PATH to the correct one and the file extension
to xacro instead of urdf. You can copy paste the code below and change the PATH and MODELNAME
to the correct one.

.. code-block:: xml

  <?xml version="1.0"?>
  <launch>
      <param name="robot_description" command="$(find xacro)/xacro '$(find testing_brubotics)/tmux_scripts/PATH/MODELNAME.xacro'" />
      
      <node name="NODENAME" pkg="gazebo_ros" type="spawn_model" output="screen"
            args="-urdf -param robot_description -model ROBOTNAME" />
            
  </launch>

Now make a second empty xacro file where we will make the "functions". Save it as FUNCTION-
NAME.xacro. In the code below an example of a macro to make a box and a joint is shown. You can
copy paste this in the file. TIP: copy paste the code from the source of overleaf.

.. code-block:: xml

  <?xml version="1.0" ?>
  <robot xmlns:xacro="http://www.ros.org/wiki/xacro">

    <xacro:macro name="m_link_box" params="name origin_xyz origin_rpy size mass ixx ixy ixz iyy iyz izz">
      <link name="${name}">
        <inertial>
          <mass value="${mass}" />
          <origin rpy="${origin_rpy}" xyz="${origin_xyz}" />
          <inertia ixx="${ixx}" ixy="${ixy}" ixz="${ixz}" iyy="${iyy}" iyz="${iyz}" izz="${izz}" />
        </inertial>
        <collision>
          <origin rpy="${origin_rpy}" xyz="${origin_xyz}" />
          <geometry>
            <box size="${size}" />
          </geometry>
        </collision>
        <visual>
          <origin rpy="${origin_rpy}" xyz="${origin_xyz}" />
          <geometry>
            <box size="${size}" />
          </geometry>
        </visual>
      </link>
    </xacro:macro>

    <xacro:macro name="m_joint" params="name type axis_xyz origin_rpy origin_xyz parent child limit_e limit_l limit_u limit_v">
      <joint name="${name}" type="${type}">
        <axis xyz="${axis_xyz}" />
        <limit effort="${limit_e}" lower="${limit_l}" upper="${limit_u}" velocity="${limit_v}" />
        <origin rpy="${origin_rpy}" xyz="${origin_xyz}" />
        <parent link="${parent}" />
        <child link="${child}" />
      </joint>
      <transmission name="trans_${name}">
        <type>transmission_interface/SimpleTransmission</type>
        <joint name="${name}">
          <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
        </joint>
        <actuator name="motor_${name}">
          <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
          <mechanicalReduction>1</mechanicalReduction>
        </actuator>
      </transmission>
    </xacro:macro>

  </robot>

Again the first lines is standard and the robot description is given between <robot> and </robot>.
In order to make a box we have to look at the first block of code. On the first line the parameters are
defined that we have to give to this function to make a box. Then the same structure can be recognized
as in the URDF file. The parameters are the following:

.. code-block:: xml

  <xacro:macro name="m_link_box" params="name origin_xyz origin_rpy size mass ixx ixy ixz iyy iyz izz">

Now we go back to the first MODELNAME.xacro that we made. We will call the function here and
for this you can copy paste the code below. The start is always the same and you have to modify the
UPPERCASE words to your example.

.. code-block:: xml

  <?xml version="1.0" ?>
  <robot name="MODELNAME" xmlns:xacro="http://www.ros.org/wiki/xacro">
      
  <!-- BGN - Include -->
    <xacro:include filename="$(find testing_brubotics)/PATH/FUNCTIONNAME.xacro" />
    <!-- END - Include -->
    
    <!-- BGN - PAYLOAD description -->
    <m_link_box name="LINKNAME"
                origin_rpy="0 0 0" origin_xyz="0 0 0.5"
                mass="1"
                ixx="0.1" ixy="0" ixz="0"
                iyy="0.1" iyz="0"
                izz="0.1"
                size="1 1 1" />
  </robot>


To communicate between the two xacro files, we have to add the line <include> with the right PATH
and name. Then we call the function <m_link_box> and give the parameters needed. When starting the
simulation with ./start.sh, you should see the box spawn in gazebo. 
Now you can make your own model.

This will only work on Ubuntu 18/Ros Melodic. If you are using ROS Noetic on Ubuntu 20, you must add xacro: before calling the m_link_box macro.

.. code-block:: xml

    <?xml version="1.0" ?>
    <robot name="MODELNAME" xmlns:xacro="http://www.ros.org/wiki/xacro">
        
    <!-- BGN - Include -->
      <xacro:include filename="$(find testing_brubotics)/PATH/FUNCTIONNAME.xacro" />
      <!-- END - Include -->
      
      <!-- BGN - PAYLOAD description -->
      <xacro:m_link_box name="LINKNAME"
                  origin_rpy="0 0 0" origin_xyz="0 0 0.5"
                  mass="1"
                  ixx="0.1" ixy="0" ixz="0"
                  iyy="0.1" iyz="0"
                  izz="0.1"
                  size="1 1 1" />
    </robot>

Starting from now all codes will be shown as this, to work on both Melodic and Noetic.

Using RVIZ
^^^^^^^^^^^

To make the correct model in the xacro file it can be long to launch everytime the gazebo simulation. A
quicker and better way is to use RVIZ for this instance. When using RVIZ the physics are not loaded like
in gazebo so it is way quicker to see the changes and how the joints are acting. For this you will have to
make a new launch file. To keep it simple name it RVIZ.launch but is can be whatever you want. Copy
paste the code from below (change the PATH and MODELNAME) and save the file.

.. code-block:: xml

  <?xml version="1.0"?>
  <launch>
      <param name="robot_description" command="$(find xacro)/xacro '$(find testing_brubotics)/tmux_scripts/PATH/MODELNAME.xacro'" />
      
    <!-- Combine joint values -->
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher"/>

    <!-- Show in Rviz   -->
    <node name="rviz" pkg="rviz" type="rviz" />

    <!-- send joint values -->
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher">
      <param name="use_gui" value="True"/>
    </node>

  </launch>

To modify the joint values and see how they change you will have to download a package. Copy paste
the following command in your terminal. Make sure to replace <your_ros_version> with the code name
of the ROS version you are using. So for Noetic, replace it with noetic! This should download the
missing package.(Normally already installed with the Droneswarm Brubotics installation.)

.. code-block:: shell

  sudo apt update
  sudo apt install ros-<your_ros_version>-joint-state-publisher-gui

Now in a terminal you can execute the command below to see your model. TIP: make sure you
spawn the objects in the origin of the plane or you will not be able to see them as RVIZ will only display a few meters away from the origin (e.g. object in 45,45,0) will not be visible).

.. code-block:: shell

  roslaunch testing_brubotics rviz.launch

This is the result you should see. 

.. figure:: _static/rviz_problem.png
   :width: 800
   :alt: alternate text
   :align: center

There is still nothing shown, this is because of the error. In the fixed
frame you need to change the "map" to the base you want
to use instead. This link will be considered the ground of your model. Take for this the "base_link" of
your model. 

Now to visualize the robot model you need:
1. Click on the add button in the left corner of the RVIZ screen
2. Search for RobotModel and click on it.
3. Click on OK
4. In this list you can also add frames.
[unclear, see video in previous note]

You should see the model now as in the following figure.

.. figure:: _static/rviz_final.png
   :width: 800
   :alt: alternate text
   :align: center

Now you can play with the joints and see how your model behaves. To see overlapping of the parts it
is possible to change the Alpha value in RobotModel to 0,5 for example and press enter. Then they are
not opaque anymore.

Instead of redoing the steps of adding a frame, change the alpha value, setting the correct frame,...
it is possible to automate this in your launch file. In rviz when all your parameters are set up, go to save
as and save it in your launch folder as "config.rviz".

Now open your launch folder and change the following line from what was there previously. You can
see that we give an argument, the config.rviz file we just made and you need to change the PATH. Save
the document and when launching again all the settings should be correct.

.. code-block:: xml

  <!-- Show in Rviz -->
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(find testing_brubotics)/PATH/config.rviz" />

Example: Creation of a bar with two cables
************************************************
.. [I would sugges to follow the youtube video instead of this example, as the expected results are easier to see on a video than in such file.]

The implementation of the following example is based on `this github code <https://github.com/massimilianop/collaborative_load_lifting/blob/master/urdf/cables_and_payload.xacro>`__. We use
this approach in order to create the joints. As it is not possible to create ball joints using xacro files, this
approach simulates ball joints by overlapping two continuous joints (one allowing a rotation around the
x-axis and one around the y-axis). This example is given to demonstrate the choice of reference in the
xacro file. The following code was written to create the system

.. code-block:: xml

  <xacro:m_link_box name="${link_00_name}"
              origin_rpy="0 0 0" origin_xyz="0 0 0.05"
              mass="0.1"
              ixx="0.1" ixy="0" ixz="0"
              iyy="0.1" iyz="0"
              izz="0.1"
              size="0.5 0.1 0.1" />
              
  <xacro:m_joint name="${link_00_name}__${link_01_name}__x" type="continuous"
           axis_xyz="1 0 0"
           origin_rpy="0 0 0" origin_xyz="0.24 0 0.1"
           parent="base_link" child="link_01"
           limit_e="1000" limit_l="-3.14" limit_u="3.14" limit_v="0.5" />
           
  <xacro:m_link_sphere name="${link_01_name}"
              origin_rpy="0 0 0" origin_xyz="0 0 0.005"  
              mass="0.01"
              ixx="0.1" ixy="0" ixz="0"
              iyy="0.1" iyz="0"
              izz="0.01"
              radius="0.01" />
              
  <xacro:m_joint name="${link_01_name}__${link_02_name}__x" type="continuous"
           axis_xyz="0 1 0"
           origin_rpy="0 0 0" origin_xyz="0 0 0"
           parent="link_01" child="link_02"
           limit_e="1000" limit_l="-3.14" limit_u="3.14" limit_v="0.5" /> 

  <xacro:m_link_cylinder name="${link_02_name}"
              origin_rpy="0 0 0" origin_xyz="0 0 0.25"  
              mass="0.01"
              ixx="0.1" ixy="0" ixz="0"
              iyy="0.1" iyz="0"
              izz="0.01"
              radius="0.01" length="0.5" />                     
            
  <xacro:m_joint name="${link_00_name}__${link_03_name}__x" type="continuous"
           axis_xyz="1 0 0"
           origin_rpy="0 0 0" origin_xyz="-0.24 0 0.1"
           parent="base_link" child="link_03"
           limit_e="1000" limit_l="-3.14" limit_u="3.14" limit_v="0.5" />

  <xacro:m_link_sphere name="${link_03_name}"
              origin_rpy="0 0 0" origin_xyz="0 0 0.005"  
              mass="0.01"
              ixx="0.1" ixy="0" ixz="0"
              iyy="0.1" iyz="0"
              izz="0.01"
              radius="0.01" />
              
  <xacro:m_joint name="${link_03_name}__${link_04_name}__x" type="continuous"
           axis_xyz="0 1 0"
           origin_rpy="0 0 0" origin_xyz="0 0 0"
           parent="link_03" child="link_04"
           limit_e="1000" limit_l="-3.14" limit_u="3.14" limit_v="0.5" />    

  <xacro:m_link_cylinder name="${link_04_name}"
              origin_rpy="0 0 0" origin_xyz="0 0 0.25"  
              mass="0.01"
              ixx="0.1" ixy="0" ixz="0"
              iyy="0.1" iyz="0"
              izz="0.01"
              radius="0.01" length="0.5" />

Explanation of code:
""""""""""""""""""""""""""""

1. The "link_00_name" represents the bar on the ground. The position of the box can be changed
with "origin_xyz", this represents the center of mass of the object.
2. For every joints, "origin_xyz" represents the position of the joint relative to the previous joint. If it is the
first joint (as for "${link_00_name}__${link_01_name}__x"), it is relative to (0,0,0).
3. For every link that is added, the "origin_xyz" will represent
the center of mass of the object relative to the previous joint. For example, "link_03_name" is
defined relative to "${link_00_name}__${link_03_name}__x"
4. Something that cannot be done in xacro files are ball joints. A solution for this is represented in
the example above. Two joints are placed in the same position to realise a rotation around both the x- and
y-axis.

To see this model, reproduce the procedure to launch it in RVIZ (see above section). If everything is working fine, you should see this:

.. figure:: _static/Example_bar_2cables.png
   :width: 800
   :alt: alternate text
   :align: center

Chaning drone initial position
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Instead of spawning the drone in the default position, you can choose where you want to spawn it. In
order to change the initial position, you will have to create a .csv file in which you specify the position at
which the drone has to be spawned. To do you, follow the following steps:

1. create a .csv file (ex: spawn_location.csv) in the directory in which you have your session.yml file "/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts"
   (you can create a .csv file using visual studio by just creating a new file and saving it as a .csv):

2. add the following line to your file and save it.

.. code-block:: xml

  1, 0.0 , 0.0 , 0.0, 0.0

Which means :

  (a) the first number = the id of the drone (if you have 1 drone, the id is 1. if you have 2 drones,
      the first drone has id 1 and the second id 2)

  (b) the following 3 numbers are the position at which you want the drone (in this case the origin)

  (c) the last number is the heading of the drone

  (d) For the case of one drone, we spawn UAV1 with id 1 in the origin (see code above) as to make
      the connection to the payload easier since we are using the link-attacher

3. add the .csv file to your session.yml by adding the following to the line containing the command to
   spawn the UAV. Change the CSV_FILE_NAME by the name of your .csv file.
   
   .. code-block:: xml

      --pos_file `pwd`/CSV_FILE_NAME.csv

   like in this example:

   .. code-block:: xml

    - waitForSimulation; rosservice call /mrs_drone_spawner/spawn "1 $UAV_TYPE --enable-rangefinder --enable-ground-truth --pos_file `pwd`/spawn_location.csv"
  
4. To change the position of multiple drones, you will have to create a .csv for each drone (don't forget
   to change the id, depending on the drone) and follow the steps above to integrate it in the session.yml file.

Making a connection between load and drone after takeoff
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. Sometimes weird behavior of the system can be observed if the connection between the drone and the
.. payload is done before takeoff. Before solving this problem, 

Another problem has to be tackled before attaching a drone and a payload precisely togheter. When performing the simulations, there is always an offset between the desired 
position of the drone and its actual position. This is because we use a regular GPS. This will result in a connection that is not perfectly
in the COM of the drone when doing the connection after takeoff. A solution is to change to a `RTK GPS <https://en.wikipedia.org/wiki/Real-time_kinematic_positioning>`__.

Use a RTK GPS
***************

To switch to a RTK GPS, two things must be done:

1. The drone must be spawned with following line in the session.yml file. This enables a publisher of
   the ground truth position of the UAV.

  .. code-block:: xml

    --enable-ground-truth

2. Following line must be added in the pre-window of the session.yml file.
   
   .. code-block:: xml

      export ODOMETRY\_TYPE="rtk"


Change in code to perform connection after takeoff
*****************************************************

To perform the connection after takeoff, the drone must follow a couple of steps:

1. make the drone takeoff without connection to the payload
2. make the drone fly above the position where you will spawn the payload
3. pause the physics of the simulation
4. spawn the payload
5. use link attacher to make a connection between the payload and the drone
6. unpause the physics

This results in a change of lines 77 to 89 in Session.yml in this `Github file <https://github.com/mrs-brubotics/testing_brubotics/blob/master/tmux_scripts/load_transportation/6_one_drone_SE3controllerBrubotics_Robustness_mv1/session.yml>`__.

.. code-block:: xml

    - load:
      layout: tiled
      panes:
        - waitForControl;
          sleep 25;
          rosservice call /uav1/control_manager/goto '[-5.0, -5.0, 1.2, 0.0]';
          sleep 15;
          rosservice call gazebo/pause_physics;
          roslaunch testing_brubotics payload_6.launch;
          sleep 2
          rosservice call /link_attacher_node/attach_typed 'uav1' 'base_link' 'bar' 'link_04' 'fixed';
          sleep 2;
          rosservice call gazebo/unpause_physics


Change tracker after take-off and take-off height
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Since the collision properties have to be deactivated in order to get two drone closer than 3m to each
other, the tracker has to be changed after take-off. To do so, a custom_configs has to be created inside
the folder in which the session.yml file resides. In this custom_configs folder, create a new file called
uav_manager.yaml and add the following:

.. code-block:: xml

  takeoff:

        after_takeoff:
            tracker: "LineTracker"
            controller: "Se3Controller"

    takeoff_height: 1

this code will change the tracker after take-off by the "Linetracker". The LineTracker allows the drone
to fly close to each other (remove the collision properties). In the same code it is also possible to change
the take-off height.
To implement this in the session.yml, the following code as to be added at the part of the control
inside the session.yml:

.. code-block:: xml

  - waitForOdometry; roslaunch mrs_uav_general core.launch DEBUG:=false
    config_uav_manager:=./custom_configs/uav_manager.yam

Change UAV mass
^^^^^^^^^^^^^^^^

In order to simulate with a hardware UAV mass (2.40 kg for f450, TODO??kg for t650) some manual changes are required in the mrs_uav_system (explained for the f450):
If this step is not done correctly, the feedforward action of the controller will create a steady state error in the z direction (i.e the drone will be higher than expected if the mass in the controller is smaller than the real one). 

* Open *~/mrs_workspace/src/simulation/ros_packages/mrs_simulation/models/mrs_robots_description/urdf/f450.xacro* and adjust the mass: *<xacro:property name="mass" value="${2.40-0.005*4.0-0.015-0.00001}" /> <!-- [kg] 2.40-->* . This ensures that Gazebo simulates a UAV 
  model with the hardware mass. Note that the xacro has slight offset from 2.4kg since afterwards some small masses (of motors, sensors) are added to the uav so we subtract them before they are added.

* Open *~/mrs_workspace/src/uav_core/ros_packages/mrs_uav_managers/config/simulation/f450/mass.yaml* and adjust the mass: uav_mass: 2.40 #2.00 # [kg]. 
  This ensures that the controllers and trackers that use mass (e.g., for feedforward actions) use th hardware mass.

* Catkin build the mrs_worspace (although not strcitly necessary if you only change configs, make a habit to catkin build more than too less)

.. note::
  Do not forget to do the above steps each time you reinstall the mrs_uav_system! For hardware experiments the UAV mass used in the controllers and trackers is the one set in the ~/.bashrc, hence the above changes do not effect operation on hardware.

* For UAVs with payload, you need to do the same for what concerns mass of only the UAV (excluding payload mass), but you also need to ensure that the xacro of the payload has the same payload mass as the one you use in 
  the controller and tracker. This is normally exported in the session.yml file of each test folder, where you have to change *LOAD_MASS:0.2* and *export LOAD_MASS=0.2* with the chosen mass. 
  For 2 UAVs each UAV offcourse compensates for half of the bar's mass instead of the total payload mass in the case of one UAV with cable suspended load. So you must put the full mass in *LOAD_MASS:m* but only half of it in *export LOAD_MASS=m/2*

.. note:: 
  For the se3_brubotics_load_controller, the mass of the UAV is loaded through the variable defined in the ~/.bashrc file as well. So changing the yaml files as explained above might not be enough.
  To solve this issue you can either change the value in the ~/.bashrc directly. Or add *export UAV_MASS="2.4"* alongside the other export in the session.yml of your test. This export will normally overwrite
  the value present in the ~/.bashrc.



Hardware setup 
-------------------

.. admonition:: todo

   Raphael todo: same as above but specifically for the hardware.
   explain the hardware valdiations of the Cable Suspended PAyload Mechanism via Matlab. 
   Not the design as this was discussed before. 
   Explain How this relates to the model used in simulation and highlight the differences.

In this part, the steps needed to validate the good operation of the Cable-Suspended Payload Module will be explained.


Off-board validation via MATLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first step is to plot in real-time the data coming from the Arduino using a matlab script (without having to deal with the BACA protocol used for communication with the ROS frame work. This will be the next step).

Prerequist
****************

To do so, you need to install the Arduino IDE following `this <https://docs.arduino.cc/software/ide-v1/tutorials/Linux>`__ link.
If an error is displayed running the installation script, you might need to run sudo ./install.sh instead of just ./install.sh
If the error keeps popping, it might be that the script cannot create the Arduino folder in /usr/local/bin. To do so, execute the following code 

.. code-block:: shell

  cd /usr/local/bin
  sudo mkdir arduino

Then go back to the installation folder and reexecute the installation script. 
Now you should find the Arduino shortcut in the Applications of Ubuntu.

Performing the test on matlab
*********************************

Once you installed the IDE, you can upload the script you want throught it. 
The script that must be compiled on the arduino can be found here : *~/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/Raphael/arduino/EMS22A_encoder/EMS22A_encoder.ino*. 
You can then click on the "V" to check the syntax (in the IDE), and then on the arrow on the right to upload it. See the following figure:

.. admonition:: todo

   Raphael: please refer to the code on github. If you need to highlight sections, then only put those in here but not the full code, not via screenshot but inline code as you did below (but better if you can explain by referring to the code, maybe mention the variable names in case this helps).

.. figure:: _static/ArduinoScript.png
   :width: 800
   :alt: alternate text
   :align: center

If no error are displayed, the correct code is now running on your arduino. See next section for the explanation of this code.

To plot in real time the data coming from the Arduino, you must be sure that the *Communication_Matlab* variable is set on *true*. If not, change it and upload it again.
Now you can close the Arduino IDE and open Matlab.

Open the scripts located in */home/nuc3/git/droneswarm_brubotics/.gitman/testing_brubotics/tmux_scripts/Raphael/arduino/matlab*.

* *Test.m* is the file you must run to open the correct port and plot the data. 

* *ReadSineWaveData.m* is a function that is plotting the data that is being received from the Arduino. It is called each time we receive a new packet. You can change the duration
  of the test by changing the value here : *src.UserData.Count > 400*. By default only one of the encoder will be plotted live. You can uncomment the part of the second encoder to have both at the same time if needed.

* *post_pocessing* is a script that can be used when the test is finish to produce nice graphs.


The data that is being plotted is the angle of each encoder and the angular velocity associated. 
Here is an example of the kind of graph that you might be able to generate with the scripts (here using *post_processing.m*):

.. figure:: _static/ArduinoGraphExample.png
   :width: 800
   :alt: alternate text
   :align: center

Explaining the Arduino code 
******************************

.. admonition:: todo

   Raphael: instead of copying the code it is better to refer to this file on github. If you need to highlight sections, then only put those in here but not the full code.

.. code-block:: arduino 

  //Smoothing window average
  #define WINDOW_SIZE 3
  bool Communication_Matlab = false; //set to true if communicating with Matlab and false to comminicate with ROS

  int INDEX = 0;
  float VALUE = 0;
  float SUM = 0;
  float READINGS[WINDOW_SIZE];
  float AVERAGED = 0;

  int INDEX_2 = 0;
  float VALUE_2 = 0;
  float SUM_2 = 0;
  float READINGS_2[WINDOW_SIZE];
  float AVERAGED_2 = 0;

  int INDEX_3 = 0;
  float VALUE_3 = 0;
  float SUM_3 = 0;
  float READINGS_3[WINDOW_SIZE];
  float AVERAGED_3 = 0;

  int INDEX_4 = 0;
  float VALUE_4 = 0;
  float SUM_4 = 0;
  float READINGS_4[WINDOW_SIZE];
  float AVERAGED_4 = 0;

  const int PIN_CS = 5;
  const int PIN_CLOCK = 6;
  const int PIN_DATA = 7;

  const int PIN_CS_2 = 2;
  const int PIN_CLOCK_2 = 3;
  const int PIN_DATA_2 = 4;

  float oldpos;
  float newpos;
  float angle;   
  float ang_velocity;

  float oldpos_2;
  float newpos_2;
  float angle_2;
  float ang_velocity_2;

  volatile int start =0;
  volatile int dt = 0;

  float offset;
  float offset_2;

  int i = 0;

  void setup() {
    Serial.begin(9600);
    pinMode(PIN_CS, OUTPUT);
    pinMode(PIN_CLOCK, OUTPUT);
    pinMode(PIN_DATA, INPUT);
    pinMode(PIN_CS_2, OUTPUT);
    pinMode(PIN_CLOCK_2, OUTPUT);
    pinMode(PIN_DATA_2, INPUT);

    digitalWrite(PIN_CLOCK, HIGH);
    digitalWrite(PIN_CS, LOW);
    digitalWrite(PIN_CLOCK_2, HIGH);
    digitalWrite(PIN_CS_2, LOW);
    
    oldpos = 0;
    newpos = 0;
    ang_velocity = 0;
    oldpos_2 = 0;
    newpos_2 = 0;
    ang_velocity_2 = 0;

    offset = -144.66;
    offset_2 = -93.67;

    //find_offset();
  
    //get the start time
    start = millis();
  }

  //byte stream[16];
  void loop() {
    // Encoder number 1
    digitalWrite(PIN_CS, HIGH);
    digitalWrite(PIN_CS, LOW);
    int pos = 0;
    for (int i=0; i<10; i++) {
      digitalWrite(PIN_CLOCK, LOW);
      digitalWrite(PIN_CLOCK, HIGH);
    
      byte b = digitalRead(PIN_DATA) == HIGH ? 1 : 0;
      pos += b * pow(2, 10-(i+1));
    }
    for (int i=0; i<6; i++) {
      digitalWrite(PIN_CLOCK, LOW);
      digitalWrite(PIN_CLOCK, HIGH);
    }
    digitalWrite(PIN_CLOCK, LOW);
    digitalWrite(PIN_CLOCK, HIGH);
    
    //convert pos [0, 1024] to angle [-180, 180] degrees
    angle = pos + offset;
    angle = (angle - 512)*(360.0/1024);
    angle = (angle * 71) / 4068.0; //convert to radians and remove offset
    // Serial.print(angle);

    //average 
    SUM = SUM - READINGS[INDEX];       // Remove the oldest entry from the sum
    VALUE = angle;        // Read the next sensor value
    READINGS[INDEX] = VALUE;           // Add the newest reading to the window
    SUM = SUM + VALUE;                 // Add the newest reading to the sum
    INDEX = (INDEX+1) % WINDOW_SIZE;   // Increment the index, and wrap to 0 if it exceeds the window size

    AVERAGED = SUM / WINDOW_SIZE;      // Divide the sum of the window by the window size for the result

    // Encoder number 2
    digitalWrite(PIN_CS_2, HIGH);
    digitalWrite(PIN_CS_2, LOW);
    pos = 0;
    for (int i=0; i<10; i++) {
      digitalWrite(PIN_CLOCK_2, LOW);
      digitalWrite(PIN_CLOCK_2, HIGH);
    
      byte b = digitalRead(PIN_DATA_2) == HIGH ? 1 : 0;
      pos += b * pow(2, 10-(i+1));
    }
    for (int i=0; i<6; i++) {
      digitalWrite(PIN_CLOCK_2, LOW);
      digitalWrite(PIN_CLOCK_2, HIGH);
    }
    digitalWrite(PIN_CLOCK_2, LOW);
    digitalWrite(PIN_CLOCK_2, HIGH);
    
    //convert pos [0, 1024] to angle [-180, 180] degrees
    angle_2 = pos + offset_2;
    angle_2 = (angle_2 - 512)*(360.0/1024); //angle in degrees
    angle_2 = (angle_2 * 71) / 4068.0; //convert to radians and remove offset

    //average encoder 2
    SUM_2 = SUM_2 - READINGS_2[INDEX_2];       // Remove the oldest entry from the sum
    VALUE_2 = angle_2;        // Read the next sensor value
    READINGS_2[INDEX_2] = VALUE_2;           // Add the newest reading to the window
    SUM_2 = SUM_2 + VALUE_2;                 // Add the newest reading to the sum
    INDEX_2 = (INDEX_2+1) % WINDOW_SIZE;   // Increment the index, and wrap to 0 if it exceeds the window size

    AVERAGED_2 = SUM_2 / WINDOW_SIZE;      // Divide the sum of the window by the window size for the result

    if (Communication_Matlab){
      Serial.print(angle);
      Serial.print(";");
      Serial.print(AVERAGED);
      Serial.print(";");
      Serial.print(angle_2);
      Serial.print(";");
      Serial.print(AVERAGED_2);
      Serial.print(";");
    }else{
      send_data(angle*1000, 0X18);
      send_data(angle_2*1000, 0X19);
    }
    
    angular_velocity(angle, angle_2);
    delay(10);
  }

  void find_offset(){
    // take the average of the first 300 data encoder 1
    for (int i = 0; i <= 300; i++){
      // Encoder number 1
      digitalWrite(PIN_CS, HIGH);
      digitalWrite(PIN_CS, LOW);
      int pos = 0;
      for (int i=0; i<10; i++) {
        digitalWrite(PIN_CLOCK, LOW);
        digitalWrite(PIN_CLOCK, HIGH);
      
        byte b = digitalRead(PIN_DATA) == HIGH ? 1 : 0;
        pos += b * pow(2, 10-(i+1));
      }
      for (int i=0; i<6; i++) {
        digitalWrite(PIN_CLOCK, LOW);
        digitalWrite(PIN_CLOCK, HIGH);
      }
      digitalWrite(PIN_CLOCK, LOW);
      digitalWrite(PIN_CLOCK, HIGH);
      offset = offset + pos;
    }
    offset = offset / 300.0;
    offset = 512.0 - offset;

    // take the average of the second 300 data encoder 1
    for (int i = 0; i <= 300; i++){
      // Encoder number 1
      digitalWrite(PIN_CS_2, HIGH);
      digitalWrite(PIN_CS_2, LOW);
      int pos = 0;
      for (int i=0; i<10; i++) {
        digitalWrite(PIN_CLOCK_2, LOW);
        digitalWrite(PIN_CLOCK_2, HIGH);
      
        byte b = digitalRead(PIN_DATA_2) == HIGH ? 1 : 0;
        pos += b * pow(2, 10-(i+1));
      }
      for (int i=0; i<6; i++) {
        digitalWrite(PIN_CLOCK_2, LOW);
        digitalWrite(PIN_CLOCK_2, HIGH);
      }
      digitalWrite(PIN_CLOCK_2, LOW);
      digitalWrite(PIN_CLOCK_2, HIGH);
      offset_2 = offset_2 + pos;
    }
    offset_2 = offset_2 / 300.0;
    offset_2 = 512.0 - offset_2;
    Serial.println(offset);
    Serial.println(offset_2);
  }

  void angular_velocity(float data, float data_2){
    oldpos = newpos;
    newpos = data;
    
    oldpos_2 = newpos_2;
    newpos_2 = data_2;
    // Find the time
    long fin = millis();
    dt =(fin - start);
    start = fin;   // sets up start for the next interrupt
    
    //calculate angular velocity
    ang_velocity = 1000 * (newpos - oldpos)/dt; // [degree/ sec]
    ang_velocity_2 = 1000 * (newpos_2 - oldpos_2)/dt; // [degree/ sec]

    //average velocity 1
    SUM_3 = SUM_3 - READINGS_3[INDEX_3];       // Remove the oldest entry from the sum
    VALUE_3 = ang_velocity;        // Read the next sensor value
    READINGS_3[INDEX_3] = VALUE_3;           // Add the newest reading to the window
    SUM_3 = SUM_3 + VALUE_3;                 // Add the newest reading to the sum
    INDEX_3 = (INDEX_3+1) % WINDOW_SIZE;   // Increment the index, and wrap to 0 if it exceeds the window size

    AVERAGED_3 = SUM_3 / WINDOW_SIZE;      // Divide the sum of the window by the window size for the result

    //average velocity 2
    SUM_4 = SUM_4 - READINGS_4[INDEX_3];       // Remove the oldest entry from the sum
    VALUE_4 = ang_velocity_2;        // Read the next sensor value
    READINGS_4[INDEX_4] = VALUE_4;           // Add the newest reading to the window
    SUM_4 = SUM_4 + VALUE_4;                 // Add the newest reading to the sum
    INDEX_4 = (INDEX_4+1) % WINDOW_SIZE;   // Increment the index, and wrap to 0 if it exceeds the window size

    AVERAGED_4 = SUM_4 / WINDOW_SIZE;      // Divide the sum of the window by the window size for the result

    if(Communication_Matlab){
    Serial.print(ang_velocity);
    Serial.print(";");
    Serial.print(AVERAGED_3);
    Serial.print(";");
    Serial.print(ang_velocity_2);
    Serial.print(";");
    Serial.println(AVERAGED_4);
    }else{
      send_data(AVERAGED_3*1000, 0X20);
      send_data(AVERAGED_4*1000, 0X21);
    }
  }


The code begin by defining all the variables and the correct pin used to read informations from the two encoders. 
It will not be commented more as it is a common procedure in Arduino codes for reading and converting values coming from encoders.

Before trusting the data, one must also add the offset to the calculated values of each encoders. This can be done by measuring the angle when the load is vertical and then use this value as the ofset.


On-board validation via ROS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. admonition:: todo

   * Raphael todo: 
   * explain the results obtained that the measured payoad state that enters your controller (the one you use in the PD control) is correct. 
   * So I would propose to request the UAV to fly a: up and forth steps in x (0 to 2 to 0 to 2 etc), circular (sin x cos y) and figure 8 (see online for equations) trajectory at and intial heading angle of 45° (as to check that the full UAV attitude is correctly accounted for). Make sure the trajectory is not too quick so the UAV can follow it well, but also fast enough so sufficiently large payload swing angles and UAV pitch roll motions are visible. Do this with a controler and ERG used as bypasstracker.
   * Make a video of the exp.
   * Plot the graphs in 3D of UAV (plot a `RGB 3D frame <https://nl.mathworks.com/help/nav/ref/poseplot.html>`__) and payload state (position) and a line between the 2. See if it looks the same as in reality.
   * I think by showing for enough experiments that it is the same you can somehow proof it.


Autonomous Flight setup
-------------------------

.. admonition:: todo

   Raphael todo: refer to the similarities (via refering to other previous sections) and differences (mention them explicitely) in setup wrt a normal autonomous flight. 


Determination of the correct motor paramters and payload paramters
--------------------------------------------------------------------

.. admonition:: todo

   * I assume your controller has an integrator in the height active with sufficient gain (use same value as default integral gains of the Se3Controller of ctu) and is configured using the motor params determined for the T650 hardware estimated by (`see ctu <https://github.com/ctu-mrs/mrs_uav_controllers/blob/master/config/uav/t650/motor_params_default.yaml>`__). Always use feedforward term to compensate total mass of uav and payload.
   * Make sure you have fully charged batteries from which you start each experiment.
   * Attach a load of mass m_l to the UAV of mass m_uav. 
   * write down m_l and m_uav and compute the total m_total = m_m + m_uav.
   * Do a test where you let the UAV with a load attached hover at a fixed position, far enough from the ground (avoid ground effect, e.g. 3m high). Make sure the load is not swinging so either tape it (strongly) to the frame or use a sufficiently long cable.
   * To know the whole thrust (or mass) range you should have tested before (with small steps what the max payload is the UAV can still takeoff safely, be carefull). Always stay 20% below when you see full throttle is not able to takeoff the UAV. Write down the last total mass for which it could takeoff and the one for which it just could not. This is the physical total thrust constraint.
   * You log (rosbag) over time (each exp about same length, e.g. 10 minutes of flight data) the battery voltage (starting from fully charged), the thrust command your controller produces (value between 0 and 1) and the UAV position in xyz. 
   * Repeat this process for increasing masses m_l (and m_l + m_uav) within (the full range of) the thrust capabilities of the UAV. Take about 4-5 total masses over the whole range (above nominal weight), one can be without any additional payload. 
   * See which thrust constant you found for the UAV by computing it as (`in ctu's file <https://github.com/ctu-mrs/uav_core/tree/master/miscellaneous/thrust_constants/uav_thrust_curve_estimation>`__). Show me using the max, avg and min value of trust you logged to compensate the same mass (because battery voltage drops over time). 
   * Plot the data voltage, pos, trust over time and a line that corresponds to hover thrust = totalmass*g.
   * Push matlab file on github and wetransfer me the matlab and rosbags. 
   * Repeat this process for both UAVs.
   * Are the min, max, avg thrust constants different from what ctu obtained (they have 2 models for T650, so check both)? How far are is the min to max for each UAV? How different is it between 2 UAVs?
   * Based on the above answers we have to make them a function of the motor voltage (as this decreases during operation).




.. admonition:: todo

   * Raphael todo: in the sections below focus on explaining how you configured to run each experiment. E.g. change this variables in the bashrc and put the drone this. and run this cript (with link). The actual resutls are for your thesis report. So focus on the practical things to reproduce the experiment and not for the scientific thesis.

Validation of swing dampening control and trajectory-predictions
--------------------------------------------------------------------

.. admonition:: todo

   * Raphael todo: obtain results for your thesis. (write in your thesis)
   * validate the controller for a sufficenly large mass (to show there is coupling and not just a small disturbance) but not too large, so you can follow some trajectories (steps up and forth, circular, figure 8). Decide the cable length.
   * For the 2 UAVs with load design some trajectories that exploit different swing modes. Start with the steps up and forth in the longitudinal direction of the load and orthogonal to it. Then validate torqion modes by.
   * BE VERY VERY CAREFULL

Single UAV case
^^^^^^^^^^^^^^^^^

Two UAVs case
^^^^^^^^^^^^^^^

Validation of trajectory-based ERGs
-------------------------------------

.. admonition:: todo

   * Raphael todo: obtain results for your thesis. (write in your thesis)
   * Make a working discrete-time ERG on the input constraints (the individual motor thrusts, not just the total thrust and the angular speed) and show that for the very aggressive trajectories (see before those you could not do in bypass mode) you can safely "filter" them using the ERG.
   * If you have time you can do obstacle avoidance test like they did last year with the tube, but first make sure the input cosntraints work on both models (1 and 2 UAVs).
   * BE VERY VERY CAREFULL

Single UAV case
^^^^^^^^^^^^^^^^^

Two UAVs case
^^^^^^^^^^^^^^^