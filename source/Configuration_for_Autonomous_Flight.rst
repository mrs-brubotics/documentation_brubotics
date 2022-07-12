Configuration for Autonomous Flight
=================================================

NUC
----

The assumption in this section is that the NUC is brand new, in order to make the explanation easier. If
it is already used it is not that big of a problem but make sure you have enough space available on the
SSD of the NUC.

Prerequist
^^^^^^^^^^

* The first thing to do is to download Ubuntu 20.04 on your NUC. This can be easily done with a bootable USB. For more details we refer to the \href{https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview}{tutorial} written by Ubuntu;

* Next, download the workspace provided by CTU-MRS by pasting the code under `I have a fresh Ubuntu install and want it quick and easy <https://github.com/ctu-mrs/mrs_uav_system#i-have-a-fresh-ubuntu-1804-and-want-it-quick-and-easy>`__ inside a terminal;

* Download the Brubotics software by following this `README.md <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/README.md>`__.

Before proceeding make sure you are able to run scripts from CTU workspace (e.g. *~/mrs_workspace/src/simulation/example_tmux_scripts/one_drone_pendulum*) and the brubotics workspace (*~/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/Raphael/0_One_Drone_f450_BruboticsDampingController*)
 

Connection with the Pixhawk
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once this is done the basic setup of the NUC is done. The steps that will follow next will make that there
is a clear connection between the NUC and the PixHawk 4, and that the NUC recognises the PixHawk 4
when it is connected to it (using always the same usb ports).

These steps will be based on `this <https://ctu-mrs.github.io/docs/hardware/px4_configuration.html>`__ tutorial written by CTU-MRS.

* Connect a mouse, keyboard and the PixHawk 4 to the Intel NUC (Use the FTDI board (TLL cable) and not not the standart USB cable of the PixHawk, see section on FTDI REF TO PUT HERE). There are 3 USB connections and you can chose
  where to put each device, but then not change after configuration is done. 
  I think we used the normal usb to micro usb cable for this, not sure. Might need to double check next time I come to the lab. Maxime do you remember ? For QGround control we used a standard usb for sure.

* Once the device is powered on (by the battery) and you are logged in, open a terminal and go to cd ../../dev or by
  clicking first on "other locations" in Files. If you type "ls" you will get a list of every connection that
  is made between the NUC and other devices or modules;

* Find which device name is associated to the PixHawk 4. You can do this by first plugging in the
  PixHawk 4, running "ls" inside the /dev folder, unplugging the PixHawk 4, running "ls" again and
  comparing both results to see which device is missing. Only look at the "tty" names, others are not
  for the PixHawk 4, (it can also be 'ttyUSB0'for the front USB of the NUC).

.. note:: 
	"serial" might be seen in the list when doing the "ls" command. But it is not the name we are looking for, so you can ignore it.

* Once the name of the device is found run the following command in the /dev folder:
  
.. code-block:: shell 

   udevadm info -p $(udevadm info -q path -n /dev/ttyUSB0) | grep 'SERIAL_SHORT\|VENDOR_ID\|MODEL_ID'

* Here is what it should looks like :

.. figure:: _static/PixHawkPortDevLs.png
   :width: 800
   :alt: alternate text
   :align: center


* Replace '/dev/ttyUSB0' by the device name associated to your PixHawk 4. If you typed it correctly you should get
  a similar result (with different numbers as mine):

.. code-block:: shell

	E: ID_MODEL_ID=6001
	E: ID_SERIAL_SHORT=A50285BI
	E: ID_VENDOR_ID=0403


* In your terminal, go to "/etc/udev/rules.d/"" and create a new file called "99-usb-serial.rules" by using
  the following command in the terminal (Skip this command if the file is already there):

.. code-block:: shell

	sudo touch 99-usb-serial.rules

* Edit the file (using sudo nano 99-usb-serial.rules) and paste the following line into the file, while changing the values according 
  to what you had at the previous step : Replace idVendor, idProduct and serial with your values, and change the OWNER name to the user
  name of your ubuntu session (or you can leave user on "mrs"). Make sure the quotation
  marks are present in the file, if they are not present the connection won't work!

.. code-block:: shell 

	SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{serial}=="A50285BI", 
	SYMLINK+="pixhawk",OWNER="mrs",MODE="0666"
  
* Go back to /dev. Unplug the PixHawk 4 and plug it back into the NUC, when you list all the
  devices available, you should see "pixhawk" now. If you do not, try to reboot the NUC, this should
  normally solve the issue.

* Now you should be able to run mavros on a new terminal:

.. code-block:: shell

	roslaunch mrs_uav_general mavros_uav.launch

If you have no processes that died and a long list of blacklisted and loaded items, then the setup is successfull.

.. figure:: _static/CorrectlySetupMavlink.png
   :width: 800
   :alt: alternate text
   :align: center

You have to repeat this procedure for the Arduino's and the RTK GPS M2reach module. 
Always make sure to use the same USB port when doing this. 
On the NUC3 the file will looks like : 

.. code-block:: shell
  
  SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", ATTRS{serial}=="0001", SYMLINK+="pixhawk",OWNER="mrs",MODE="0666"
  SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0043", ATTRS{serial}=="85937313737351503252", SYMLINK+="arduino",OWNER="vub",MODE="0666"
  SUBSYSTEM=="tty", ATTRS{idVendor}=="3032", ATTRS{idProduct}=="0013", ATTRS{serial}=="8243EDAF73DFD683", SYMLINK+="rtk",OWNER="mrs",MODE="0666"
  SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0043", ATTRS{serial}=="7593231393835130E061", SYMLINK+="arduino",OWNER="vub",MODE="0666"


SSH Configuration
^^^^^^^^^^^^^^^^^

Another problem that needs to be solved is what concerns the ssh service of the
NUC. As a safety measure, this service is disabled each time the NUC reboots so we need to enable
it again before flying, otherwise it would not be possible to remotely login into the NUC and start the
shell script for the experiment. When typing ’sudo systemctl status ssh’ and you get the same results
as in 

.. figure:: _static/SSHCouldnotbefound.png
   :width: 800
   :alt: alternate text
   :align: center


You first have to do :

.. code-block:: shell

  sudo apt-get install ssh

If get the same result as the following pictures **after** rebooting completeley the nuc and running the same command, you can skip the next parts as the SSH is already launched automatically.

.. figure:: _static/SShActiveAfterBoot.png
   :width: 800
   :alt: alternate text
   :align: center
  

But if you get the same result as there : 

.. figure:: _static/SSHExpectedBootProblem.png
   :width: 800
   :alt: alternate text
   :align: center


In order to enable the ssh again a monitor, mouse and keyboard is needed and of course it is not very handy to do on the drone's NUC each time you want to make a test. 
To address this issue a shell script is created that will start the ssh service automatically when the NUC is turned on. 
Here is the procedure to follow to correct this : 

* Follow the steps of How to install SSH server in Ubuntu (only the top parts before step 1) of this link.

* Create a new file in */etc/* using :

.. code-block:: shell
  
  sudo touch /etc/rc.local

* Go inside this file

.. code-block:: shell

  sudo nano /etc/rc.local

* Paste the following in that file : 

.. code-block:: shell
  
  #!/bin/sh
  sudo systemctl ssh start
  exit 0

.. note::

  If it doesn't work it may be due to the second line. Change it to "sudo service ssh start".

* Make the script executable by running :

.. code-block:: shell

  sudo chmod +x /etc/rc.local

* Reboot to see if it worked. Now when you open a terminal and type :

.. code-block:: shell
  
  sudo systemctl status ssh

You should now get the same result as on the following figure :


.. figure:: _static/SShActiveAfterBoot.png
   :width: 800
   :alt: alternate text
   :align: center


Wireless Connection to the onboard NUCs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To be able to remotely control the nuc by SSH into it from a base computer, one needs to configure a wifi router.

**Connect to internet with the router**


The first essential things is to have internet access when connected to the router via Wifi. 

To do so, one must follow these steps :

* Power on the router and plug an Ethernet cable in the router’s Internet port. If you are at the Lab in the building Z these are located on the walls. 
* Connect your device to the router’s Wi-Fi network. Use the 2.4Gz only as the 5Gz gives problems later on with the GPS. (The password of the wifi is written at the back of the router)
* Go on the router’s website http://192.168.0.1 (usrname and psw: "admin")
* Go to Quick Setup, Wireless Router, Static Ip and fill in all required information of your network.
 
If you are at VUB, here are the settings you have to put to connect to the network :

.. figure:: _static/RouterIPconfigVUB.png
   :width: 800
   :alt: alternate text
   :align: center
   

You should now have internet over the router's wifi with your NUC. If it's not the case check if the ethernet port of the wall is working fine (or just test another one.)

**Configure the static IP of each connected device**

Once every PC can access internet on router rename all IP addresses as follows and set Netmask to 255.255.255.0.
The ip of the ground station must be 192.168.0.100, while the IP of the NUC's must be 192.168.0.10X, with X being the number of the NUC.

* Go to WiFi settings, connect to the routers network
* select the router network and under "Details" you find the IPv4 address and the Hardware address corresponds to the MAC address. 
* To change the IP, you go to the IPv4 tab, set to Manual instead of Automatic, and set the IP address and netmask to the value described above. 

.. figure:: _static/IPv4SettingsCorrectNUC.png
   :width: 800
   :alt: alternate text
   :align: center

Let DNS server on automatic. PROBLEM IN WINDOWS : must be set, to what?? Was still able to ping and SSH into the nuc but I lose internet. Doesn't seems to be an issue on ubuntu, so 
with the third NUC Bryan will bring it should be okay. (and the section will be confirmed).

Then check via ifconfig if the ip adress is set now correctly:

You can find back the device IP address and MAC address on Ubuntu by typing ifconfig and get as output the inet (IPv4) and the ether (Mac
address) (make sure you connected to the router network) :

.. figure:: _static/ifconfigCorrectIP.png
   :width: 800
   :alt: alternate text
   :align: center

The last one is the information corresponding to the NUC, meaning that it's configured correctly.

One also might need to change the MAC adress of each computer in the router's website itself.it was needed for my windows computer, but might 
already be configured for the other nuc's, as their MAC adress are the same as before normally.
TO CHECK ADD pictures of the corresponding tab. 


Config RTK
----------


What we did not modified :
  Github issue : https://github.com/ctu-mrs/mrs_uav_system/issues/77 we let gga to 1hz instead of 10 as explained in the issue.
  Changed the TCP parameters , figure 4.31 rover of the tutorial. See screenshots taken friday 13/05.
  Figure 4.33 is showing different parameters from what has been stated above. (POSITION OUTPUT)


The Real-Time Kinematic (RTK) system is composed of the Emlid Reach RS2 as the "base" an the Emlid
Reach M2 attached to the drone as the "rover". To the latter is connected the Multi-band GNSS antenna.
The RTK is a GPS-based positioning system that allows to get cm-precise XYZ position from Global
Navigation Satellite System (GNSS) measurements. The base and rover setup will help to get the RTK
precision. Simply explained, the RTK system consists of the base (i.e. Reach RS2), the device that doesn’t
move, and the rover (i.e. Reach M2), the device attached to the UAV. Both devices individually can get
GNSS measurements with usual GPS precision. The RTK system computes the baseline, the difference
between both measurements, which gives the rover’s position relative to the base.

.. admonition:: todo

   Still need to add the pictures of the correct parameters + small explanations. Normally everything was already okay and well configured so should be fast to just transpose.
   /!\ separate well the rover and base part to avoid confusion. It was not that clear in overleaf. 


Create launch scripts and configure the MRS code
------------------------------------------------
This section will cover the different files and parameters that must be configured prior to launching a test on hardware. Might be good to print this and the next section "Autonomous flight procedure" to have it easilly available on site, and to check each point before lauching any test (i.e. as a check list before takeoff).
Before doing anything, check that all the workspaces build correctly and that the code are up to date. Additionnal advices can be found `here <https://ctu-mrs.github.io/docs/system/preparing_for_a_real-world_experiment.html>`__, in MRS tutorial. Always use this tutorial when something seems unclear and update this one with the additionnal informations you needed.

Several things have to be modified in the default code from MRS to work with the hardware presented here. Except indication, all the files are in packages from MRS, located in *~/mrs_workspace/src/uav_core/ros_packages*

* **Configuration file for the RTK** Go to the `config file <https://github.com/ctu-mrs/mrs_uav_odometry/blob/master/config/uav/rtk.yaml>`__ of the rtk and change "altitude_estimator: "HEIGHT" to "altitude_estimator:
  "RTK"; 

.. figure:: _static/AltitudeEstimator.png
   :width: 350
   :alt: alternate text
   :align: center

.. admonition:: todo

  Looks like RTK was already in the available parameters, so this step might be useless. but to be sure it started with the RTK I changed it anyway.


* **In case an error regarding the baudrate is experience when launching RTK node** (normally MRS solved the issue): Go to the `launch file <https://github.com/ctu-mrs/mrs_serial/blob/master/launch/rtk.launch>`__ (you can find it here : *~/workspace/src/mrs_serial*)of the rtk and modify your baudrate according to the baudrate of the reach m2
  (and NOT reachS2) that you’ve set up in previous section "Config RTK". Sometimes even when this baudrate is specified
  and correct you can obtain an error when launching the rtk launch file. This error says that your
  baudrate is unsupported and gives you a random number. If you want to bypass this error you will
  have to impose your baudrate in the `nmea_parser.cpp <https://github.com/ctu-mrs/mrs_serial/blob/master/src/nmea_parser.cpp>`__ file and add this line after the parameters are
  loaded;

    .. figure:: _static/BaudrateRTK.png
      :width: 600
      :alt: alternate text
      :align: center



* **Bashrc configuration** : The name of the UAV has to be changed. This variable defines the UAV’s namespace, all the ROS nodes of the MRS UAV System will run under the namespace /$UAV_NAME/node_name. The UAV_NAME should match the /etc/hostname of the onboard computer.
  In our case this is looks like ""nuc3-NUC10i7FNK". This should be changed in the *~/.basrc* in home folder of the nuc. Then it must be changed as well in `config file <https://github.com/ctu-mrs/mrs_uav_general/blob/master/config/uav_names.yaml>`__ of the uav names. Delete all the names present in the robot_names list and 
  put the names of all the drones you are using. For more informations about the bashrc file and its parameters, checkout this part of the `tutorial <https://ctu-mrs.github.io/docs/system/bashrc_configuration.html#what-is-basrc>`__ of MRS.
  The mass of the UAV must also be changed to fit the one of your UAV. Same comment for the type of UAV and odometry type. Here is the correct looking section of the Bashrc file :

  .. figure:: _static/BashrcConfigAutonomous.png
    :width: 800
    :alt: alternate text
    :align: center

.. admonition:: todo

  Bryan : Do you prefer to use uav1(2,3,..) as name and change hostname of the nuc (as they need to be the same). Or should we use the current default hostname of the nuc (nuc3-NUC10i7FNK) straight away ? Looks cleaner to use uav1 but dont know if it can create other problems to change the PC name. (By changing it in etc/hostname)

.. note:: 

  Before launching any script, double check that every .bashrc file is correct for every drone. This is very important as contrary to the simulation, no environmental variable will be overwritten in Session.yalm files. 
  In addition to that, a precise planning of each test that are going to be made must be done BEFORE the test day. Each different test folder must be prepared, the code reviewed and ready. Simulations must work perfectly as well before doing hardware test. This is essential to not waste time on site changing parameters and trying to debug software issues. 


* **Shell script to lauch a test:** Create your custom tmux shell script in your test folder or use the simple `just_flying.sh <https://github.com/ctu-mrs/uav_core/blob/master/tmux_scripts/just_flying.sh>`__ script from MRS as a start. This is the equivalent of the Session file for the simulation part.
  You'll put there all the nodes that you need to launch to perform your test, as well as the custom configs related (e.g. lauching your controller with the correct parameters). As in simulation you'll also devide all nodes i
  Add the following line for the RTK GPS:

    .. code-block:: shell 
      
      'rtk' 'WaitForRos; roslaunch mrs\_serial rtk.launch'
      
  In Bryan folder here is the line that there is for the launch of the rtk.

    .. code-block:: shell 

      'rtk_serial' 'waitForRos; roslaunch mrs_serial rtk.launch baudrate:=9600

  .. admonition:: todo

    I guess it solves the issue with the baudrate without having to manually modify it as explained above ?

  Indicate the name of the project, e.g "One_drone_validation_encoder" and also indicate the MAIN_DIR where the bag files of the test will be saved.
  
  .. figure:: _static/ShellScriptNAmeAndMainDir.png
    :width: 500
    :alt: alternate text
    :align: center




* **Modifying the shell script for load controller/tracker:** As the controllers need additionnal parameters to work, these need to be exported as well. 
  * The following lines has to be added before the "input" section of the shell script :

  .. code-block:: shell

      # following commands will be executed first in each window
      pre_input="export LOAD_MASS=0.0954; export CABLE_LENGTH=0.75; export LOAD_GAIN_SWITCH=false; mkdir -p $MAIN_DIR/$PROJECT_NAME"

    The mass of the load, the cable length and the LOAD_gain switch  are set here (true means that the controller will be the load damping controller, false will be the the regular se3copy controller).
    Make sure to only put environmental variables that will be changed often between tests there, and keep the standard ones that will remains identical (e.g. Name, mass of uav, type of odometry, etc) in the bashrc where they should not be modified often.  
    Make sure to not touch the end of the shell script, after the "DO NO MODIFY BELOW" comment. This should be already well configured.

  * Add also this line among the other nodes to launch the code of the Arduino via its launch file.

  .. code-block:: shell
 
    'encoder' 'waitForRos; roslaunch testing_brubotics arduino.launch
    '

* **Add trajectory**: In order to ask a trajectory to the drone (e.g. a step in all 3 directions), one must create a txt file with the trajectory encoded in it. 
  This can be done by adding the following lines in the input of the tmux session (Always change the name of the folders accordingly to your folders and files):

  .. code-block:: shell

      'goto_start' 'WaitForRos; roslaunch testing_brubotics load_trajectory.launch file:=tmux_scripts/load_transportation/1_one_drone_validation_encoder/trajectories/movement1_uav1.txt; rosservice call /'"$UAV_NAME"'/control_manager/goto_trajectory_start
    '
      'start_challenge' 'waitForRos; history -s rosservice call /'"$UAV_NAME"'/control_manager/start_trajectory_tracking
    '
  
  As "history -s" is present, you'll have to navigate to the correct tmux tab to launch this trajectory when needed. To generate these .txt files, follow : TODO ADD EXPLANATIONS FOR THIS.
  Does this trajectory must be relative to the RTK, or to the take-off/initial position of the drone ??
  
When your shell script is ready, try to launch it (remotely to test as well the network) without making the drone take-off to see if no error is displayed. Errors can easilly happens if indentation and spaces are not consistent, so this must be checked several times to ensure that no problem will occur during a real take-off.


* **Custom configurations** In your folder where the just_flying.sh template is pasted, create a folder custom_configs where you will put your yaml files to overwrite
  the parameters from the differents launch files. The yaml files you need are :

    * `world_hardware.yaml <https://github.com/ctu-mrs/mrs_uav_general/blob/master/config/worlds/world_simulation.yaml>`__ : add the actual lat-long coordinates of the BASE in the utm_origin_lat-long
      part. This will ensure the right computation of the baseline. Be as precise as you can on the lat
      long value. This has to be done everytime you move the RTK base, and to be double checked everytime before making the drone take off, as it might be dangerous. 

    * `rtk_republisher.yaml <https://github.com/ctu-mrs/mrs_uav_odometry/blob/master/config/rtk_republisher.yaml>`__ : not necessary but if you plan to use all the topics related to the rtk, the
      offset x-y should be the latlong coordinates of the base CONVERTED in UTM coordinates. Useless ? Not done in Bryan's folder.

    * Odometry.yalm should contain all the changes made to your odometry parameters (w.r.t the default values set by MRS here `mrs_uav_odometry/config <https://github.com/ctu-mrs/mrs_uav_odometry/blob/master/config/>`__ and 
      more particularly in *default_config.yaml* where you can choose the estimator you want. 

      .. code-block:: xml

        lateral_estimator = 'RTK'
        altitude_estimator = 'RTK'
        altitude :
        use_rtk_altitude = true
      
      You can also play with the Q and R matrices of the altitude and latitude estimator. For more
      information about the Kalman filter, read the `Wikipedia page <https://en.wikipedia.org/wiki/Kalman_filter>`__. But here, remember than if you
      want in the odometry to put the emphasis more on the RTK measurements, just reduce the value
      of the R of the height_rtk and pos_rtk. Add the following lines on your odometry.yaml :
      
      .. code-block:: xml
      
        altitude :
        R:
        height_rtk: [0.01]
        lateral :
        R:
        pos_rtk: [0.01]
      
      To go further, you can also disable the fusing operation by disabling the fusion of the vel_baro
      measurement in the altitude_estimator.yaml but this is unsafe.

    * `uav_manager.yaml <https://github.com/ctu-mrs/mrs_uav_general/blob/master/config/default/uav_manager>`__ : To set up the takeoff height as desired and put the `max_thrust <https://github.com/ctu-mrs/mrs_uav_general/blob/master/config/default/uav_manager#L71>`__ to 1 to avoid
      most of undesired elandings.

      .. figure:: _static/ExampleUavManagerConfig.png
        :width: 800
        :alt: alternate text
        :align: center

      Be sure to allow the overwriting by adding in your custom scripts the config and link it to the right
      custom config file :


    * Make sure that the config files you make are loaded in the start shell script, and overwrite the default parameters.

      .. code-block:: shell

          'Control' 'waitForRos;
          roslaunch controllers_brubotics controllers_brubotics.launch custom_config_se3_copy_controller:=custom_configs/gains/hardware/se3_copy.yaml custom_config_se3_brubotics_controller:=custom_configs/gains/hardware/se3_brubotics.yaml;
          roslaunch trackers_brubotics trackers_brubotics.launch custom_config_dergbryan_tracker:=custom_configs/gains/dergbryan.yaml;
          roslaunch mrs_uav_general core.launch WORLD_FILE:=custom_configs/world_hardware.yaml config_control_manager:=custom_configs/control_manager.yaml config_uav_manager:=custom_configs/uav_manager.yaml config_odometry:=custom_configs/odometry.yaml config_constraint_manager:=custom_configs/constraint_manager.yaml config_se3_controller:=custom_configs/gains/hardware/se3.yaml config_motor_params:=custom_configs/motor_params_hardware.yaml
        '
      
      As some parameters are not the same in the simulation and the hardware tests, put the custom configs files in another folder than the one used for simulation, and double check that you load the correct one in both situations. 

.. admonition:: todo

  (Comment that was in the overleaf : [For the moment, the offset in Z is weird and the current solution is to
  add an 66.75 offset in the odometry.cpp.] See if it will be necessary to do the same this year or if we do not have this issue.)


.. admonition:: todo

  following part is redundant with what is written in the next part "Autonomous flight procedure". Delete it as soon as the part is validated and complete. 

With this all done, follow those steps when your UAV is outside: 

* First wait for the RTK FIX. You can see it in the EMLID ReachView of the Reach M2. Just access
  it by typing its IP address on your browser
  Figure 4.36: Look at the RTK Status at the top right corner in the EMLID ReachView App on your
  browser (possible in the app also)

* Launch the .sh script

* Wait for the convergence to the current altitude of the drone. It takes more or less 10 seconds

* Arm the drone with the C switch (down position) and put it the the desired flight mode with the A
  switch (UP = manual, Middle = ALTCTL, DOWN = POSCTL)

* Put the drone in offboard mode with the B switch (down position). The drone will takeoff automatically.

* Now you can send it to a setpoint with a rosservice command or through the status tab
  Note that each battery can withstand more or less 2 flights. So prepare well your experiment. Make
  sure the batteries are at 16.8V (fully charged for 4S) before you start to fly. When the battery voltage is
  close to 14 V, it is better to not take off in order to avoid damage to the batteries. This can be changed
  in the px4_config.yaml BUT you definitely shouldn’t change this value.

Cable-Suspended Payload Module
------------------------------

.. admonition:: todo

   Raphael: Explain all you need to configure the module.

Arduino setup
^^^^^^^^^^^^^^^

Configure the NUC to recognize the Arduino port
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To be sure that the Arduino is recognized by the NUC everytime it is plugged in, one must do the following steps :

Once the Arduino is correctly connected to the computer using the lower USB port at the back of the nuc, it will show up as something similar to /dev/ttyUSB0. 
To find what port is used type the following command and use this name for the next command in the terminal : 

.. code-block:: shell

  ls -l /dev/ttyACM*

This should give the port to which the Arduino Uno is connected. Replace in the next
command the correct port and paste it in the terminal : 

.. code-block:: shell

  udevadm info -p $(udevadm info -q path -n /dev/ttyACM0) | grep 'SERIAL_SHORT\|VENDOR_ID\|MODEL_ID'

This should return the an information similar to what can be seen here under (Values might be different): 

.. code-block:: shell 

    E: ID_MODEL_ID=0043
    E: ID_SERIAL_SHORT=757363033363518031F0
    E: ID_VENDOR_ID=2341

Then create a new file (or edit it if you already done this part for the Pixhawk or for the RTK Gps) in /etc/udev/rules.d/ and call it 99-usb-serial.rules. Paste the fol-
lowing line in this text document and change it with the information obtained by using
previous command : 

.. code-block:: shell 

  SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0043", ATTRS{serial}=="757363033363518031F0", SYMLINK+="arduino",
  OWNER="vub",MODE="0666"

To validate that this link has been done correctly, connect the arduino to its USB port and go to the folder /dev then type ls in a terminal opened there. It should display "Arduino" in the list of device.

In the mrs serial package a new launch file should be created for example arduino.launch
with the correct baudrate and port:

.. code-block:: xml

  <launch>

    <arg name="UAV_NAME" default="$(optenv UAV_NAME uav)" />
    <arg name="name" default="" />
    <arg name="portname" default="/dev/ttyACM0" />  <!-- INPUT : Put the correct port for the Arduino -->
    <arg name="baudrate" default="9600" /> <!-- INPUT : Put the correct baudrate for the Arduino, should be 9600 if using the same script -->
    <!-- "/dev/arduino" baudrate: 9600 19200 38400 57600 115200 230400 460800 500000 576000 921600-->
    <arg name="profiler" default="$(optenv PROFILER false)" />

    <arg name="swap_garmins" default="$(optenv SWAP_GARMINS false)" />

    <!-- will it run using GNU debugger? -->
    <arg name="DEBUG" default="false" />
    <arg unless="$(arg DEBUG)" name="launch_prefix_debug" value=""/>
    <arg     if="$(arg DEBUG)" name="launch_prefix_debug" value="debug_roslaunch"/>

    <!-- will it run as standalone nodelet or using a nodelet manager? -->
    <arg name="standalone" default="true" />
    <arg name="manager" default="$(arg UAV_NAME)_bacaprotocol_manager" />
    <arg name="n_threads" default="8" />
    <arg unless="$(arg standalone)" name="nodelet" value="load"/>
    <arg     if="$(arg standalone)" name="nodelet" value="standalone"/>
    <arg unless="$(arg standalone)" name="nodelet_manager" value="$(arg manager)"/>
    <arg     if="$(arg standalone)" name="nodelet_manager" value=""/>

    <group ns="$(arg UAV_NAME)">

      <!-- launch the nodelet -->
      <node pkg="nodelet" type="nodelet" name="serial" args="$(arg nodelet) baca_protocol/BacaProtocol $(arg nodelet_manager)" launch-prefix="$(arg launch_prefix_debug)" output="screen">

        <param name="uav_name" type="string" value="$(arg UAV_NAME)"/>

        <rosparam file="$(find mrs_serial)/config/mrs_serial.yaml" />

        <param name="enable_profiler" type="bool" value="$(arg profiler)" />
        <param name="portname" value="$(arg portname)"/>
        <param name="baudrate" value="$(arg baudrate)"/>
        <param name="use_timeout" value="false"/>

        <param name="swap_garmins" value="$(arg swap_garmins)"/>

        <!-- Publishers -->
        <remap from="~range" to="/$(arg UAV_NAME)/garmin/range" />
        <remap from="~range_up" to="/$(arg UAV_NAME)/garmin/range_up" />
        <remap from="~profiler" to="profiler" />
        <remap from="~baca_protocol_out" to="~received_message" />

          <!-- Subscribers -->
        <remap from="~baca_protocol_in" to="~send_message" />
        <remap from="~raw_in" to="~send_raw_message" />

      </node>

    </group>

  </launch>


It is then possible to do roslaunch and subscribe to the topic in a new terminal using the following two commands : 

.. code-block:: shell

  roslaunch mrs_serial arduino.launch
  rostopic echo /uav1/serial/received_message

This can, as usual be automated in a shell script file.

BACA Protocol in Arduino code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To use the encoder among the ROS framework, one has to use the `BACA protocol <https://github.com/ctu-mrs/mrs_serial>`__ to send the relevant data via the USB port of the arduino, to the NUC.
The following function is implemented in the Arduino to correctly transform the data and send it to ROS.
Then a node will be able to subscribe to a specific topic to read this data flow, and use it for measuring the load's position.
Here is the full function used :

.. code-block:: arduino

  //communication with ROS
  void send_data(int16_t data, int16_t message_id) {
    uint8_t checksum = 0;
    uint8_t payload_size = 3;

    byte bytes[2];
    //split 16 bit integer to two 8 bit integers
    bytes[0] = (data >> 8) & 0xFF;
    bytes[1] = data & 0xFF;

    //message start
    Serial.write('b');
    checksum += 'b';

    //payload size
    Serial.write(payload_size);
    checksum += payload_size;

    //payload
    Serial.write(message_id); // message_id
    checksum += message_id;

    Serial.write(bytes[0]);
    checksum += bytes[0];

    Serial.write(bytes[1]);
    checksum += bytes[1];

    //checksum
    Serial.write(checksum);
  }

The message is defined as below:

.. code-block:: cpp

  ['b'][payload_size][payload_0(=message_id)][payload_1]...[payload_n][checksum]

Between each brackets, there is one eight bit value. The message starts with the
character "b". Then the size of the message is defined in the next eight bit value. This
represents how long the transferred data is. The message id is then next, to differentiate
the various sensors. Finally the last byte is the checksum. This is calculated as follows:

.. code-block:: arduino

  uint8_t checksum = 'b' + payload_size + payload0 + payload1 + payload_n

This checksum is calculated and put to the end of the message. ROS calculates this checksum again
and compares to see if it is the same. In case there is a difference, the data was not
transferred correctly and the message is discarded. 

To enable the communication with ROS, one must change the first line of the code to switch from "MATLAB communication" to "Ros communication"

.. code-block:: arduino

  bool Communication_Matlab = false; //set to true if communicating with Matlab and false to comminicate with ROS

In the controller and trackers code, one can subscribe to the topic : "/*UAVNAME$/serial/received_message" to get the data coming from the BACA protocol. 
This has not been tested more yet, a test will probably be made at VUB asap. I think the folder *https://github.com/mrs-brubotics/testing_brubotics/tree/master/tmux_scripts/load_transportation/1_one_drone_validation_encoder*
was made for this by last year students, but it is probably already flying. There is probably a way to launch the BACA protocol without having to fly the drone (even with the standard non-damping controller). 
 
Raphael : Remaining parts to transpose are "4.14.4 Modifying the MRS code", "4.15 Making the drone take off and fly", "4.16 Set up the Nimbro parameters according to MRS" 
maybe the part about take off and fly is redundant with the Hardware.rst written already in this tutorial. Check before doing it.


