9. Config RTK
=============
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