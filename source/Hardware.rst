.. _?. Hardware:

?. Hardware
============

.. role:: raw-html(raw)
    :format: html

?.? Introduction
-----------------


?.? Step-by-step Procedure
--------------------------


?.?.? RTK Field setup
---------------------
  * Connect the ground station computer to the WiFi of the router.
  * Setup the base at the desired location and make sure it is perfectly leveled.
  * Power the base.
  * After about 2 minutes you can browse on the base URL 
  * Once connected to the base, in the 'base mode' tab, base coordinates section, make sure to set the coordinate accumulation time to 5 minutes. Wait untill the process bar is 100% finished.
  * Put the drone at a desired location and power up the Pixhawk and the on-board computer.
  * Wait a few minutes for the gps beep and click on gps 'SWITCH' knob.
  * Wait 1-5 minutes and try to browse on the rover URL. The issue is that Reach M2 rover cannot connect quickly: 
      * some students claim it connects faster on some computers;
      * try rebooting the Reach M2 by replugging the usb cable;
      * be patient :-)
  * Wait for the rover to go from status SINGLE to FLOAT, and finally to FIX. The better the sky visiblity the faster you get a FIX.
  * From the ground station computer ssh on the on-board computer (i.e. ssh nuc5@<hostname or ip>).
  * cd to your custom configs and sudo nano world_simulation.yaml.
  * Change the coordinates utm_origin_long and utm_origin_lat to the value of the base position you see inside the rover tab. This should be the about same value as the one in the base mode of the base tab.
  * In the above process you only need to repeat the steps concerning the rover (not the base if it is not powered off) each time you plug out the drone's batteries.
  
  
?.?.? Run an experiment
---------------------
  * From the ground station computer ssh on the on-board computer (i.e. ssh nuc5@<hostname or ip>).
  * cd to your testing folder.
  * Run the command tmux kill-server to be sure no other session are active.
  * Run your tmux scripts for the real hardware (not simulation), i.e. ./ TAB TAB ENTER.
  * Yuo should know that for exiting the session you can run: ctrl + c and then ctrl + a and k. If you directly do ctrl + a and k, you will have to ssh again into to the on-board computer.
  * Check if any tabs crashed. Move accross all tabs by ctrl + a and n. If crashes occured try restarting the session, if they remain crashed, solve the issue.
  * The status should show stable and reasonable odometry values for the odometry source you use. Do not continue when this is not the case!
  * We advise for each new test moment and location to do the folowng steps once: move the drone above the base and check if the x and y should be 0 with 5cm error for RTK. The RTK z value of the drone should be about 20cm when on te ground and about 1.5m when above the base. Now find out the x and y axis of the world frame and mark them (e.g. [0 5] and [5 0]. These axes will always be parallel independent of the orientation of the base. Measure the distance from these 2 points till base. This should be 5m with a 5cm error for RTK.
  * DANGER: if the status tab displays strong oscillations or jumps in the odometry values do not continue. This issue can also suddenly occur during flight, the safety pilot should immediately land the drone in this case. Pontetial solutions (no guarantees):
      * Check if the pixhawk gps is still blinking green (fix). Click on the gps 'SWITCH' knob to make it fix. 
      * Start the session again.
      * Check if the baudrates are set correctly.
      * Replug the Pixhawk.
      * Replug the Reach M2 and follow the standard steps to get RTK FIX.
  * When you have an RTK fix, on the right of the status tab should be displayed 'L1_int'.
