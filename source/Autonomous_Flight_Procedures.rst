Autonomous Flight Procedures
=============================

.. admonition:: todo

   see main todo in the previous chapter. update this when you do the hardware tests.

Step-by-step procedure for preparations prior to autonomous hardware experiments
----------------------------------------------------------------------------------
* Prepare the simulations and testing scripts;
* Prepare the scripts for the real hardware tests in the same folder as the simulation scripts;
* Make a planning for the test day (write out what you plan to do as task, what you will measure, how you will post-process data, ... and explain what yuo expect to obtain as results);
* Charge all devices: RC transmitter, RTK base, UAV batteries, your phone, your laptop, video camera,...;
* Prepare the hardware for the transport to the testing location;
* Always perform tests with at least two piots: one safety pilot handling the RC transmitter, one ground station pilot handling the ground station PC;
* Only plan test days when the wheater (sky visibility, wind) allows safe testing.
    

Step-by-step procedure during autonomous hardware experiments
--------------------------------------------------------------

RTK system setup
^^^^^^^^^^^^^^^^^^^^^^^
* Install all devices without powering those that are drained quickly.
* Connect the ground station computer to the WiFi network of the router.
* Setup the base at the desired location and make sure it is perfectly leveled.
* Power on the base.
* After about 2 minutes you can browse on the base URL on the ground station computer.
* Once you can browse the base URL, in the 'base mode' tab, base coordinates section, make sure to set the coordinate accumulation time to 5 minutes. Wait untill the process bar is 100% finished.
* Put the UAV at a desired location (based on the next experiment you will run) and power up the Pixhawk and the on-board computer by connecting the battery.
* Wait a few minutes for the pixhawk GPS beeps and click on gps 'SWITCH' knob.
* Wait 1-5 minutes and try to browse on the rover URL. 
  
  .. admonition:: note

    The issue is that Reach M2 rover cannot connect quickly. It might depend on which comuter you have. Try rebooting the Reach M2 by replugging the usb cable. Be patient.

* Wait for the rover to go from status SINGLE to FLOAT, and finally to FIX. The better the sky visiblity the faster you get a FIX.
* From the ground station computer ssh into the on-board computer, (i.e. ssh nucID@<hostname or ip>).
  
  .. code-block:: shell 

    ssh nucID@<hostname or ip>

* cd to the testing folder of the test you want to do now.
* cd to your custom configs and type

  .. code-block:: shell 

    sudo nano world_simulation.yaml


* Change the coordinates utm_origin_long and utm_origin_lat to the value of the base position you see inside the rover tab. This should be the about same value as the one in the base mode of the base tab. Do not move the base from now on or you will have to change the coordinates.
* Repeat the above steps for each of the rovers you plan to test with.
* In the above process you only need to repeat the steps concerning the rover (not the base if it has not moved or been powered off) each time you plug out the UAV's batteries. 
  
  
Run an autonomous hardware experiment
----------------------------------------
* At the start of each new test moment, first always run the basic examples (i.e., just_flying_rtk.sh and the last script you tested before that worked).
* From the ground station computer ssh on the on-board computer (i.e., ssh nuc5@<hostname or ip>).

  .. code-block:: shell 

    ssh nucID@<hostname or ip>

* cd to your testing folder.
* Run the following command to be sure no other session are active:

  .. code-block:: shell 

    tmux kill-server

* Run your tmux scripts for the real hardware, i.e. ./ TAB TAB ENTER.
* You should know that for exiting the session you can use: ctrl + c and then ctrl + a and k. If you directly do ctrl + a and k, you will have to ssh again into to the on-board computer and lose time.
* Check if any tmux tabs crashed. Move accross all tabs by ctrl + a and n. If crashes occured try restarting the session, if they remain crashed, solve the issue.
* The status window should show stable and reasonable odometry values for the odometry source you use (e.g., GPS, RTK GPS, ...). Do not continue when this is not the case for sufficiently long times!
* We advise for each new test moment and location to do the folowing steps once: move the UAV above the base and check if the x and y should be 0 with 5cm error for RTK. The RTK z value of the UAV should be about 20cm when on the ground and about 1.5m when above the base. Now find out the x and y axis of the world frame and mark them (e.g., [0 5]m and [5 0]m. These axes will always be parallel independent of the orientation of the base. Measure the distance from these 2 points till base. This should be 5m with a 5cm error for RTK.
* DANGER: if the status tab displays strong oscillations or jumps in the odometry values do not continue. This issue can also suddenly occur during flight, the safety pilot should immediately land the UAV in this case. Pontetial solutions (no guarantees):
    * Check if the pixhawk gps is still blinking green (fix). Click on the gps 'SWITCH' knob to make it fix. 
    * Start the session again.
    * Check if the baudrates are set correctly.
    * Replug the Pixhawk.
    * Replug the Reach M2 and follow the standard steps to get RTK FIX.
* When you have an RTK fix, on the right of the status tab should be displayed 'L1_int'.
* When the odometry is correct the safety pilot can takeoff the UAV. 
      * The same procedure as for manual flight with some differences.
      * After the UAV is ARMED and the props start spinning, toggle the OFFBOARD switch. Immediately set the throttle to a value of the hover thrust (e.g. 57%). The latter is important when taking back manual control so the UAV does not fall down. After a few sconds the thrust will increase and the UAV will takeoff. 

