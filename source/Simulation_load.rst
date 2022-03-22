Step to reproduce spawning of a load, still need to adapt syntax
===================================================================

CHECK FOR PICTURES TO ADD AND SMALL MODIFICATIONS IN THE CONTENT 

Creation of a new world with payload and automate it to start with simulation
-----------------------------------------------------------------------------

To create a new simulation environment, follow these steps:

* Make a new folder in the directory: "/workspace/src/droneswarm\_brubotics/ros\_packages/testing\_brubotics/tmux\_scripts"
* Copy the files ("start.sh", "layout.json" and "session.yml") of an other example (from brubotics or mrs) in your new folder. Take a simulation as close as possible to 
  what you want to achieve (e.g.:one drone gps). 
* Launch your simulation, by opening terminal and typing "./start.sh". 
* If you want to create a new world \bc{see previous chapter on creating new worlds, this "general" explanation belongs there}, copy an existing world file in in the folder 
  you created in ".../tmux\_scripts" . The location of the standard mrs world files are in following directory:\\  "/git/simulation/ros\_packages/mrs\_gazebo\_common\_resources". For our situation, we started from the standard grass plane. \bc{be more specific about which files you base yourself on for the specific task you have.}\pp{Done}
* Adapt your file by adding models, e.g. boxes or trees. We added a box that will be used as payload. Find exemples of codes to implement this models in other .world files or 
  the internet. \bc{no no need list this code if you do not talk about specific parts in the code. Better to insert a link to this file on our github. Try to explain which parameters 
  are important to set for your purpose. Now it is again "quite generic". It does not directly explain the specifics, although they might be in the code.}\pp{Done, not pushed yet so 
  we still have to add link}
* Change "world\_name:=grass\_plane" in the following lines in the "Session.yml" file:

.. code-block:: xml

  - gazebo:
    layout: tiled
    panes:
    - waitForRos; roslaunch mrs_simulation simulation.launch world_name:=grass_plane gui:=true
    to (adapt text in LARGE LETTERING)
  - gazebo:
    layout: tiled
    panes:
      - waitForRos; roslaunch mrs_simulation simulation.launch world_file:='/home/NAME OF COMPUTER SESSION/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/NAME OF FOLDER/NAME OF WORLD FILE.world' gui:=true

Link_attacher
-------------

Installation
^^^^^^^^^^^^
The required package is normally installed directly with the dronesware brubotics installation. Check in ~/workspace/src/ if mrs_gazebo_extras_resources is present or not.
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
^^^^^^^^^^^^^^^^^^
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

Then create another file called "box.launch" and copy paste the following code inside (if you've chosen to extend the .world file, you can skip this part, see next section):

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
you will see the payload is not implemented yet in the control and will oscillate.

Model your payload with an URDF file
------------------------------------

Instead of spawning the box in the world file as done previously, it is possible to make an urdf file of the
payload. This has the advantage that you can define more comlex connections of multiple objects and
add joints between elements.

Create urdf file
^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^
Now that you have created the urdf file, it needs to be executed. Therefore we use a launch file. Again
open a blank file and save it as NAME.launch, with "NAME" that can be what you want.Place
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
to your NAME.launch file). You should see a box spawn like on the figure.

.. code-block:: shell

  roslaunch testing_brubotics NAME.launch

Automate this using tmux
^^^^^^^^^^^^^^^^^^^^^^^^

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
-------------------------------------
The advantage with using xacro files is that we can use macros. This means that instead of defining each
link in the urdf file we can make a macro. A macro acts line a function were we give variables and this
makes a link. This means that we use 2 xacro files, one where the "functions" are defined and one were
the parameters are given. Because the number of files begins to increase, sub folders are made to have a
clearer overview like on the figure below. Later the files will be put in the right folder and pushed to the
brubotics github.

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

To communicate between the two xacro files, we have to add the line <include> with the right PATH
and name. Then we call the function <m_link_box> and give the parameters needed. When starting the
simulation with ./start.sh, you should see the box spawn in gazebo. Now you can make your own model.

Using RVIZ
----------

To make the correct model in the xacro file it can be long to launch everytime the gazebo simulation. A
quicker and better way is to use RVIZ for this instance. When using RVIZ the physics are not loaded like
in gazebo so it is way quicker to see the changes and how the joints are acting. For this you will have to
make a new launch file. To keep it simple name it RVIZ.launch but is can be whatever you want. Copy
paste the code from below (change the PATH and MODELNAME) and save the file. TIP: Copy paste it
from the source code of overleaf.

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
of the ROS version you are using. So for Melodic, replace it with melodic! This should download the
missing package.(Normally already installed with the Droneswarm Brubotics installation.)

.. code-block:: shell

  sudo apt update
  sudo apt install ros-<your_ros_version>-joint-state-publisher-gui

Now in a terminal you can execute the command below to see your model. TIP: make sure you
spawn the objects in the origin of the plane or you will not be able to see them as RVIZ will only display a few meters away from the origin (e.g. object in 45,45,0) will not be visible).

.. code-block:: shell

  roslaunch testing_brubotics rviz.launch

This is the result you should see. There is still nothing shown, this is because of the error. In the fixed
frame you need to change the "map" [you should put your window in full screen] AD to the base you want
to use instead. This link will be considered the ground of your model. Take for this the "base_link" of
your model. PICTURE HERE

Now to visualize the robot model you need:
1. Click on the add button in the left corner of the RVIZ screen
2. Search for RobotModel and click on it.
3. Click on OK
4. In this list you can also add frames.
[TO MODIFY NOT CLEAR, see youtube video]

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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
I would sugges to follow the youtube video instead of this example, as the expected results are easier to see on a video than in such file.
