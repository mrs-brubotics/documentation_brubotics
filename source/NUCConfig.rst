8. Configuration of the NUC for autonomous flight
=================================================

The assumption in this section is that the NUC is brand new, in order to make the explanation easier. If
it is already used it is not that big of a problem but make sure you have enough space available on the
SSD of the NUC.

8.1 Prerequist
--------------

* The first thing to do is to download Ubuntu 20.04 on your NUC. This can be easily done with a bootable USB. For more details we refer to the \href{https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview}{tutorial} written by Ubuntu;

* Next, download the workspace provided by CTU-MRS by pasting the code under `I have a fresh Ubuntu install and want it quick and easy <https://github.com/ctu-mrs/mrs_uav_system#i-have-a-fresh-ubuntu-1804-and-want-it-quick-and-easy>`__ inside a terminal;

* Download the Brubotics software by following this `README.md <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/README.md>`__.

Before proceeding make sure you are able to run scripts from CTU workspace (e.g. *~/mrs_workspace/src/simulation/example_tmux_scripts/one_drone_pendulum*) and the brubotics workspace (*~/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/Raphael/0_One_Drone_f450_BruboticsDampingController*)
 

8.2 Connection with the Pixhawk
-------------------------------

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


8.3 SSH Configuration
---------------------

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


8.4 Connection to the onboard NUCs
----------------------------------

To be able to remotely control the nuc by SSH into it from a base computer, one needs to configure a wifi router.

8.4.1 Connect to internet with the router
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

8.4.2 Configure the static IP of each connected device
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
