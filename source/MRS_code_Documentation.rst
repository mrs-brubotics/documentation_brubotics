MRS code documentation
======================

In this section you'll learn how to install, update, and use the UAV system from CTU Prague.

Introduction to MRS
-------------------

The `Multi-Robot Systems (MRS) UAV system of the <https://ctu-mrs.github.io/>`__ `Prague Multi-Robot Systems Group's <http://mrs.felk.cvut.cz/>`__  is a multi-robot software platform for 
simulating and experimenting with aerial robots. Various software modules for UAV navigation, estimation, control, vision, localization, mapping, etc, and tools such 
as `ROS <https://www.ros.org/>`__ , `Gazebo <http://gazebosim.org/>`__ , and `Vim <https://www.vim.org/>`__ , etc, can be used. This platform is built 
to allow safe verification of approaches in aerial robot planning, estimation, control or computer vision for instance.
Since this report's main purpose is documenting - in a tutorial style (similar to `ROS for robotics programming second edition <https://ctu-mrs.github.io/docs/introduction/>`__ )the MRS software 
platform (on top of the `MRS wiki <https://ctu-mrs.github.io/docs/introduction/>`__) and its extensions (developed by researchers of Brubotics) for the validation of novel *constrained and 
optimal control techniques* applied to aerial robot swarms, we will mainly focus the documentation of the available control and navigation modules. 

Installation
^^^^^^^^^^^^
After going through the `MRS wiki <https://ctu-mrs.github.io/docs/introduction/>`__  and installing the `MRS UAV system <https://github.com/ctu-mrs/mrs_uav_system>`__ , three folders can be found in your home directory:

* git
* mrs_workspace
* workspace

.. note::
     we advise to start installing from a fresh Ubuntu 20.04 partition.

.. note::
     The installation will also install some hidden folders. You can see them by clicking on "show hidden files". (or use <CTRL + H)

.. note::
     The installation will also update your .bashrc script, which is ran whenever you open a new terminal, to include the \workspace and \mrs_workspace folders to the system path.

The git folder includes the source code for the system. It contains the cloned repositories `simulation <https://github.com/ctu-mrs/simulation>`__ , `uav_core <https://github.com/ctu-mrs/uav_core>`__ and
 `example_ros_packages <https://github.com/ctu-mrs/example_ros_packages>`__ , `mrs_uav_system <https://github.com/ctu-mrs/mrs_uav_system>`__, `linux-setup <https://github.com/klaxalk/linux-setup>`__ and 
 it does not have binaries or other build files. The folders mrs\workspace and workspace are catkin (ROS) workspaces that use these repositories as sources. They are separate folders because a git repository should 
 not include build files and binaries, which are generated automatically and may be different for each computer. The idea is that the MRS group updates the mrs_workspace and non-MRS researchers who are using the MRS 
 software can extend it by working in the workspace folder (which can be version controlled using git see \ref subsec:Update_MrsTODO}), the contents of these files are detailed in \ref subsec:MRS_file_systemTODO}.

.. note:: 
     After installation of the MRS software, you will possibly get problems when doing git push (even in newly created empty git repositories). The cause of this problem is unclear, but it can 
     be avoided by following  `these steps to setup SSH keys <https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>`__. 

Update the MRS software using git and gitman
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The different folders of the MRS github are managed using `gitman <https://ctu-mrs.github.io/docs/software/gitman.html>`__. So `uav_core <https://github.com/ctu-mrs/uav_core>`__ and 
`simulation <https://github.com/ctu-mrs/simulation>`__ do not directly contain the *ros_packages* but a reference that is linked by `gitman <https://ctu-mrs.github.io/docs/software/gitman.html>`__ thanks to a .gitman.yaml script. 
This makes it possible to work on each packages as it it was a different github folder.
`This <https://github.com/ctu-mrs/uav_core/blob/master/.gitman.yml>`__ gitman.yml you can find an example from `uav_core <https://github.com/ctu-mrs/uav_core>`__.

.. code-block:: yaml
    
    location: .gitman
    sources:
        - name: #name of the git folder you want to add
        type: git
        repo: #https of the git folder you want as submodule
        sparse_paths:
        -
        rev: #branch of the git you want to refer -usually master-
        link: #where you want to pull your folder in the current git folder
        script: 
            - git submodule update --init --recursive
        - name:...#repeat for every submodule you want


Normally if you are in `uav_core <https://github.com/ctu-mrs/uav_core>`__ or `simulation <https://github.com/ctu-mrs/simulation>`__, *git pull* command should be enough to update the 
folder and the packages in it, because mrs group remap this command to pull every submodule in any folder that contains *gitman.yalm* files by using gitman install instead \bc I don't agree, better to do what's in caption of figure with pull al script}.
Check if your master branch is up to date with origin/master by executing the *git status* command. 

.. \begin figure}[H]
.. \begin center}
.. \includegraphics[width=14cm] Chapter_MRS_code_documentation/Pics/git_status_uptodate.png}
.. \caption What you should get after executing   git pull} and   git status}. You should regularly update via   git pull} the packages in the paths:   home/mrs_workspace/src/simulation} (git checkout master branch of each package to check if updated),   home/mrs_workspace/src/uav_core/ros_packages} (via   ./pull_all.sh}) and
..   /workspace/src/example_ros_packages} (git checkout master branch of each package to check if updated)}
.. \end center}
.. \end figure}

If the update does not work properly, you may be missing a file or updated one. Please refer to the `MRS documentation <https://ctu-mrs.github.io/docs/introduction/how_to_update.html>`__.
In fact you can also execute *gitman update* or pull all packages separately by going inside (*cd pathtopackage*) and execute the *git pull* command.
There is also in `uav_core/ros_packages a *pull_all.sh* <https://github.com/ctu-mrs/uav_core/tree/master/ros_packages>`__ script that pulls automatically each package from `uav_core <https://github.com/ctu-mrs/uav_core>`__. 
There is no similar script in `simulation <https://github.com/ctu-mrs/simulation>`__ , so each of the simulation packages should be pulled manually.

.. note:: 
    We recommend to update the MRS workspace everyday due to the constant updates by the CTU-MRS team. Be careful about the updates as some of these could change the code you developed last time. 
    If you downgraded some package to stable commits, do not pull or it'll not be stable anymore.

Disclaimer: You may obtain error messages after executing *git pull*. This usually happens if you made modifications in the MRS folders. Please remove your unnecessary modifications by using *git clean -n* } and do *git clean -f* as explained `here <https://koukia.ca/how-to-remove-local-untracked-files-from-the-current-git-branch-571c6ce9b6b1>`__, then you can pull again. 
If it is not working, you can also execute *git reset - -hard origin/master* in the modified folder before executing *git pull*.
Or better yet, put your tests in the *workspace* folder, as explained in Chapter \ref TODOch:MRSSoftwareExtensions, and try to not modify mrs_workspace as much as possible. In case you do discuss with Bryan why you need to do so. 
If you want to start from scratch (e.g. a bug you do not get solved), you can also rerun the `installation script <https://github.com/ctu-mrs/mrs_uav_system>`__, it will automatically update the different folders and do not rewrite the ones that are up to date.

Compiling the workspaces
^^^^^^^^^^^^^^^^^^^^^^^^
To compile each workspace, you must open a terminal in the workspace folder and do the following command :

.. code-block:: shell

     catkin build 

If your screen freeze during the compilation, you can add *-j2* to avoid it.

Sometimes warnings can occur after building. Just rebuild again and that should build without any warnings.
This should be done for the mrs_workapce first and them for workspace folder
ADD 2 photos here of warnings

.. note::
     The two packages are linked, you  have to build mrs_workspace before doing anything on workspace. 

Understanding the MRS file system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ROS packages from `mrs_workspace/src/uav\core/ros\packages <https://github.com/ctu-mrs/uav_core>`__ :

The level of importance for the purpose of navigation and control is set to [low], [medium] or [high].

* `mavros <https://ctu-mrs.github.io/docs/software/uav_core/mavros/>`__ [medium]:add specific communication node for UAV control, thanks to `MAVLink <https://mavlink.io/en/>`__ .  
* `mrs_bumper <https://ctu-mrs.github.io/docs/software/uav_core/mrs_bumper>`__ [low]: aggregates data from 1-D, 2-D lidars, and depth-camera images and creates a sector-based representation of the UAV surroundings.
* `mrs_lib <https://ctu-mrs.github.io/docs/software/uav_core/mrs_lib>`__ [medium]: contains various useful libraries. A more detailed documentation on C++ classes and namespaces is available `here <https://ctu-mrs.github.io/mrs_lib/>`__.
* `mrs_mavros_interface <https://ctu-mrs.github.io/docs/software/uav_core/mrs_mavros_interface>`__ [medium]: contains the MavrosDiagnostics node which reports changes of the UAV state (arming, offboard, ...) and published diagnostics messages.
* `mrs_msgs <https://ctu-mrs.github.io/docs/software/uav_core/mrs_msgs/>`__ [high]: contains all the definitions of the custom (i.e. non standard) messages (.msg) and services (.srv) used by the publisher/subscriber/service/server nodes of the system. A more detailed documentation is available `here <https://ctu-mrs.github.io/mrs_msgs/index.html>`__. This is why there is no msg and no srv folder in any of the other packages. \bc a question for in next chapter: how and where will you add your custom messages? they say you should do it in that folder.}\mf Indeed, if we need to add new messages it will be in this folder}

 Important .msg:

* `AttitudeCommand.msg <https://ctu-mrs.github.io/mrs_msgs/msg/AttitudeCommand.html>`__.
* `ConstraintManagerDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/ConstraintManagerDiagnostics.html>`__.
* `ControlError.msg <https://ctu-mrs.github.io/mrs_msgs/msg/ControlError.html>`__.
* `ControlManagerDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/ControlManagerDiagnostics.html>`__.
* `EspOdometry.msg <https://ctu-mrs.github.io/mrs_msgs/msg/EspOdometry.html>`__.
* `EstimatorType.msg <https://ctu-mrs.github.io/mrs_msgs/msg/EstimatorType.html>`__.
* `EulerAngles.msg <https://ctu-mrs.github.io/mrs_msgs/msg/EulerAngles.html>`__.
* `GainManagerDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/GainManagerDiagnostics.html>`__.
* `GpsData.msg <https://ctu-mrs.github.io/mrs_msgs/msg/GpsData.html>`__.
* `Heading.msg <https://ctu-mrs.github.io/mrs_msgs/msg/Heading.html>`__.
* `LandoffDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/LandoffDiagnostics.html>`__.
* `LkfStates.msg <https://ctu-mrs.github.io/mrs_msgs/msg/LkfStates.html>`__.
* `MavrosDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/MavrosDiagnostics.html>`__.
* `MpcTrackerDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/MpcTrackerDiagnostics.html>`__.
* `PositionCommand.msg <https://ctu-mrs.github.io/mrs_msgs/msg/PositionCommand.html>`__.
* `Reference.msg <https://ctu-mrs.github.io/mrs_msgs/msg/Reference.html>`__.
* `ReferenceList.msg <https://ctu-mrs.github.io/mrs_msgs/msg/ReferenceList.html>`__.
* `ReferenceStamped.msg <https://ctu-mrs.github.io/mrs_msgs/msg/ReferenceStamped.html>`__.
* `So3Gains.msg <https://ctu-mrs.github.io/mrs_msgs/msg/So3Gains.html>`__.
* `SystemDiagnostics.msg <https://ctu-mrs.github.io/mrs_msgs/msg/SystemDiagnostics.html>`__.
* `TrackerConstraints.msg <https://ctu-mrs.github.io/mrs_msgs/msg/TrackerConstraints.html>`__.
* `TrajectoryReference.msg <https://ctu-mrs.github.io/mrs_msgs/msg/TrajectoryReference.html>`__.
* `UavState.msg <https://ctu-mrs.github.io/mrs_msgs/msg/UavState.html>`__.

Important .srv:

* `GazeboApplyForce.srv <https://ctu-mrs.github.io/mrs_msgs/srv/GazeboApplyForce.html>`__.
* `TrackerConstraintsSrv.srv <https://ctu-mrs.github.io/mrs_msgs/srv/TrackerConstraintsSrv.html>`__.

* `mrs_optic_flow <https://ctu-mrs.github.io/docs/software/uav_core/mrs_optic_flow>`__ [low]: gets velocity measurement of a UAV using on-board sensors.  
* `mrs_rviz_plugins <https://ctu-mrs.github.io/docs/software/uav_core/mrs_rviz_plugins>`__ [low]: contains plugins for Rviz visualization tool.
* `mrs_uav_controllers <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_controllers>`__ [high]: receives a reference from  `mrs_uav_trackers <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_trackers>`__ and controls the states of the UAV by outputting desired angular rate (or desired orientation) and thrust. The SO3 controller is implemented.
* `mrs_uav_general <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_general>`__ [high]: contains all necessary configuration files, launch files and utilities
* `mrs_uav_managers <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_managers/>`__ [high]: divided into 5 managers 
    
* ControlManager: Defines 3 classes *ControllerParams*, *TrackerParams* and *ControlManager* the main class which allow the tracking and the control of UAV according to safety parameters.
* UavManager: Defines the class *UavManager*, which deals with takeoff and landing process.
* ConstraintManager: Defines the class *ConstraintManager* which allows for setting system constraints.
* GainManager: Defines the class *GainManager* which assigns gains to UAV.
* Null_tracker: Define the class *NullTracker* which initializes the tracker used when the system is landing.
* `mrs_uav_odometry <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_odometry>`__ [medium]: provides estimation of the
     * lateral position
     * lateral velocity 
     * lateral acceleration 
     * heading
     * heading rate
     
of the UAV thanks to sensors. The results are used by the `ControlManager <https://github.com/ctu-mrs/mrs_uav_managers>`__. for the `Controllers <https://github.com/ctu-mrs/mrs_uav_controllers>`__ of the UAV, and by the `Constraint/Gain Manager <https://github.com/ctu-mrs/mrs_uav_managers>`__ for dynamics constraints and controller gains.
    * `mrs_uav_status <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_status>`__ [medium]: provides a control interface which allows the user to control the UAV, call ROS services, monitor the nodes and display custom messages from them, monitor the topic rates
    * `mrs_uav_testing <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_testing>`__ [medium]: provides automated simulation tests of the uav core, such as gps control test, bumper test and collision avoidance test. You can run the tmux scripts.
    * `mrs_uav_trackers <https://ctu-mrs.github.io/docs/software/uav_core/mrs_uav_trackers>`__ [high]: receives references from a navigation software and generates feasible references which satisfy a set of state constraints to controllers. Several trackers can b eused such as the MpcTracker (advanced) and LineTracker (basic).


ROS packages from `mrs_workspace/src/simulation/ros_packages <https://github.com/ctu-mrs/simulation>`__:

The simulation folder contains all useful packages for the Gazebo simulation and some tmux scripts for an automated simulation launch. 


* `mavlink_sitl_gazebo <https://github.com/ctu-mrs/px4_sitl_gazebo>`__ [high]: This package contains all the Gazebo files for a SITL (software in the loop) simulation using Pixhawk. Pixhawk offers open source flight control software for vehicles such as drones.
* `mrs_gazebo_common_resources <https://github.com/ctu-mrs/mrs_gazebo_common_resources>`__ [medium]: This package contains Gazebo resources for the simulation, such as worlds, models and plugins. The readme files contain more detailed explanation of the plugins.    
* `mrs_simulation <https://github.com/ctu-mrs/mrs_simulation>`__ [high]: It allows the user to spawn vehicles into the Gazebo simulation and to select from multiple UAV types which can be equipped with different sensors. After launching the simulation, the user can spawn a UAV with the   spawn_uav} command. Then the vehicle's model is spawned using the PX4 flight control and mavros for nodes communication. It allows also to perform a `distributed simulation <https://github.com/ctu-mrs/mrs_simulation#distributed-simulation >`__ across multiple machines. 
* `px4_firmware <https://github.com/ctu-mrs/px4_firmware>`__ [high]: The PX4 firmware is necessary to use Pixhawk for the Gazebo simulation.



ROS packages from `git/example_ros_packages/ros_packages <https://github.com/ctu-mrs/example_ros_packages>`__ :

* `example_ros_pluginlib <https://github.com/ctu-mrs/example_ros_pluginlib>`__ [high]: Contains *Plugin*, *ExampleManager*, *PluginParams* and *ExamplePlugin* classes. Gives an example of creation and management of the pluginlib feature of ROS.
* `example_ros_uav <https://github.com/ctu-mrs/example_ros_uav>`__ [high]: Contains *WaypointFlier -public nodelet-* class. Gives an example of UAV simulation with the good coding practice and example features nodelet initialization, subscriber, publisher and timer initialization, using   mrs_lib},...-. The   README} is full of advice about using ROS. \bc good tutorial to check}
* `example_ros_vision <https://github.com/ctu-mrs/example_ros_vision>`__ [low]: Contains *EdgeDetect -public nodelet-* class. Gives an example of how to use OpenCV and deal with computer vision from embedded camera.



Topics and Services
A list with the important topics and services used through the MRS system is discussed `here <https://ctu-mrs.github.io/docs/system/uav_ros_interface.html>`__.

