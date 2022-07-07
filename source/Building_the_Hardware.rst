Building the Hardware
=====================
This chapter explains which components are to be ordered and and how to build a custom UAV from its different components.
We provide details for the development of two distinct UAV versions based on the frame size: an F450 and a T650 UAV.
Moreover, we provide the details of additional hardware modules that extends the functionality of the UAVs.
To know which UAV and additional hardware can be used for which type of experiments, please read 

.. admonition:: todo

   Bryan refer to other section(s).

.. admonition:: todo

   Bryan add for all componenets a link of where there can be ordered


F450 UAV
--------

.. admonition:: todo

   Bryan complete section(s).


T650 UAV
--------

Components
^^^^^^^^^^
This section lists all the components required to build the UAV.

Off-the-shelf
*************

* **Frame**: the body of the UAV, to which all other components are fixed.

  *Choice*: the Tarot Ironman 650 frame.

  .. figure:: _static/frame.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Tarot Ironman 650 frame

  .. admonition:: note

     We do not use the feet of the frame but instead 3D print custom feet as to reserve more space for additional hardware (e.g., an encoder mechnism for payload transport) at the bottom part of the UAV.

  .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive white borders)

* **Flight Controller (FC)**: the FC is an electronic board with sensors (e.g., accelerometer, gyroscope, barometer, GPS) required for controlling the attitude (and position) of the UAV.
  
  *Choice*: the Pixhawk 4 flight controller, which comes with its own GPS sensor and power distribution board.

  .. figure:: _static/PX4.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Pixhawk 4 FC

* **Power Distribution Board (PDB)**: the PDB is the central board of the UAV where the power supplied of most of the the electrical components converge and are connected to the battery.

  *Choice*: PDB delivered with the Pixhawk 4.

  .. figure:: _static/PDB.jpg
     :width: 400
     :alt: alternate text
     :align: center

     Pixhawk 4 PDB

  .. admonition:: todo

     replace with higher resolution picture

* **GPS**: retrieves the absolute (i.e., global) position of the UAV.

  *Choice*: the GPS from the Pixhawk 4.

  .. figure:: _static/gps.jpg
     :width: 400
     :alt: alternate text
     :align: center

     Pixhawk 4 GPS

  .. admonition:: todo

     replace with higher resolution picture and make sure the picture cropped (as to delete the grey bottom reactangle)

* **Motors**: when a voltage is supplied these rotate their propellors at a desired speed command proportianal to the applied voltage.
  
  *Choice*: two pairs of the Tarot 4114 320KV Multi-Rotor brushless motors.

  .. figure:: _static/motor.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Tarot 4114 320KV Multi-Rotor brushless motor
  
  .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive grey borders)

* **Propellers**: move the air due to the motor's motion and prdocue a thrust force that move the UAV.

  *Choice*: two pairs (CW and CCW) of the Tarot 15X5.5 Carbon Fiber Propeller TL2831

   .. figure:: _static/propeller.jpg
      :width: 800
      :alt: alternate text
      :align: center

      Tarot 15X5.5 Carbon Fiber Propeller TL2831

* **Electronic Speed Controllers (ESCs)**: are electronic circuits used to control the speed of the motors.

  *Choice*: for each motor a Turnigy MultiStar BLheli_32 ARM 51A Race Spec ESC 2~6S.

   .. figure:: _static/esc.jpg
      :width: 800
      :alt: alternate text
      :align: center

      Turnigy MultiStar BLheli_32 ARM 51A Race Spec ESC 2~6S

   .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive grey borders)

* **Battery**: the battery powers all electrical components on the UAV and is typically on of the heaviest components on the UAV. It is recommended to buy enough spare batteries.

  *Choice*: a Turnigy Graphene Professional 12000mAh 6S15C LiPo Pack.

   .. figure:: _static/battery.jpg
      :width: 800
      :alt: alternate text
      :align: center

      Turnigy Graphene Professional 12000mAh 6S15C LiPo Pack

   .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive grey borders)

* **RC Receiver (RCR)**: the RCR is a device that allows unidirectional wireless communication with the UAV. It receives and sends information from/to the RC transmitter that is located off-board the UAV.

  *Choice*: Hitec Optima SL

  .. figure:: _static/optima.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Hitec Optima SL

.. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive grey borders)

* **RC Transmitter (RCT)**: the RCT is held by a human operator and teleoperates the UAV (i.e., it sends toggle and joystick commands to the UAV and receives some limited on-board information). This can be used to manually fly the UAV or as a safety control that overtakes autonomous flight if the operated sees somethings goes wrong.

  *Choice*: Hitec Flash 8

  .. figure:: _static/hitec.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Hitec Flash 8

  .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive grey borders)


* **Other**: 

   Electrical cables: 
      * 20 x this used for that
      * 10 x this used for that

   Electrical connectors: 
      * 3 x this used for that
      * 5 x this used for that
   
   Mechanical connectors (screw, bolts and nuts): 
      * 3 x this used for that
      * 5 x this used for that
   
   Other?:
      * 3 x this used for that
      * 5 x this used for that


  .. admonition:: todo

     add here all screws, nuts, and their sizes, and other things like tape, straps, jumper cables, soldering iron, cables (which type of cable thickness and flexible), connectors (all yellow connectors or metal connectors to power things) and explain for what these are used.
     Old text of Maxime I placed here: All the holes used to attach something directly on the frame are for M3 bolts and the other holes M2.5 bolts. Use M3x12mm and M2.5x12mm bolts.


.. admonition:: note

   The components listed above are all you need to build a UAV for manual flight. Optionally, if one wants to build a UAV for more advanced autonomous flight, one will need some of (your choice) the additional components listed below.

* **Companion computer**: the main computational unit on-board the UAV, used to compute most estimation, planning and control algorithms in real-time.

  *Choice*: the Intel NUC BXNUC10i7FNK2.

   .. figure:: _static/nuc.jpg
      :width: 800
      :alt: alternate text
      :align: center

      Intel NUC BXNUC10i7FNK2

* **FTDI cable**: the FTDI cable is a USB to Serial (TTL level) converter which allows for a simple way to connect TTL interface devices to USB. The I/O pins of this FTDI cable are configured to operate at 5V. It basically enables the Pixhawk and the companion computer to communicate.

  *Choice*: To check correct name

  .. admonition:: todo

     Bryan

  .. figure:: _static/ftdi.jpg
     :width: 800
     :alt: alternate text
     :align: center

     TODO ADD CORRECT NAME

* **DC-DC converter**: enables to provide the appropriate supply voltage to the companion computer which is typically in a different range of voltage/current/power as the battery.

  *Choice*: Wingoneer XL4016E1 (input: 4-40V, output: 1.25-36V at max 8A, max power: 200W). Since the 6S LiPo battery (i.e., a Turnigy Graphene Professional 12000mAh 6S15C LiPo Pack) provides at most 25.2V and at least 18.0V and the Intel NUC companion computer needs a supply voltage between 12V and 19V and has a rated power supply of 19V/6.33A, the converter must be able to take 18V-25.2V in and deliver 19V/6.33A (or 121W).

    .. admonition:: todo

     add links to datasheets onlien and cite the pages where you found this info. 

    .. admonition:: todo

     add link where you ordered this convertor

  .. figure:: _static/converter.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Wingoneer XL4016E1

* **Real-Time Kinematic (RTK) GPS**: RTK is a GPS-based positioning system that allows to get more precise (i.e., cm-precise) global (in XY and Z) position from Global Navigation Satellite System (GNSS) measurements. It is used additionality to the standard GPS senor on-board the UAV which typically only obtain m-level precision. The RTK system typically consists of a stationary ground "base" station that sends corrections to an RTK module on-board the UAV which is called the "rover". Both devices individually can get GNSS measurements with usual GPS precision. The RTK system computes the baseline, the difference between both measurements then gives the rover’s position relative to the base.

  *Choice*: the Emlid Reach M2 UAV Mapping Kit. It is composed of the Emlid Reach RS2 as the base and the Emlid Reach M2 attached to the UAV as the rover. To the latter is connected via a cable the Multi-band GNSS antenna.
 
   .. figure:: _static/rtk.jpg
      :width: 800
      :alt: alternate text
      :align: center

      Emlid Reach M2 UAV Mapping Kit

Custom-made
************
In this section all custom made parts to build the autonomous UAV are explained.

.. admonition:: todo

   Once the final designs are finished of both your thesis, I will need the inventor files and all stl files of the UAV (also for the F450 from which you started). I will put them on a drive that people can download it. We cannot put it on github since too large files. 
   They are accessible here (TO DO).

We 3D print all these pieces with 20% infill:

* **Main piece (x 1)**: used to provide enough space for all the components. The PDB is fixed on its lower stage, the Pixhawk and its middle stage and the Intel NUC on its top stage. 

  .. figure:: _static/pb_stage.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Main piece 

* **Pixhawk case (x 1)**: used to fix the Pixhawk on the Main piece.

  .. figure:: _static/pixhawk_case.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Pixhawk case

* **NUC case and cover (x 1 per piece)**: the inside of the case is used to fix the Intel NUC. The side part of the case is used to fix the Emlid Reach M2. The case cover is used to atached multiple other components to the Upper case.

  .. figure:: _static/Nuc_cad.jpg
     :width: 800
     :alt: alternate text
     :align: center

     NUC case and cover

* **Upper case (x 1)**: used to fix the RC receiver, the Pixhawk GPS, and the RTK Multi-band GNSS antenna.

  .. figure:: _static/upper_part.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Upper case

  When the Main piece, the Nuc case and cover, and the Upper case are assembled one gets:

  .. figure:: _static/Top.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     All the cases assembled

* **Motor top and bottom fixation (x 4 per piece)**: used to attach the motors to the frame and to fix the legs.

  .. figure:: _static/motor_fixation_top.jpg
     :width: 800
     :alt: alternate text
     :align: center
   
     Motor top fixation

  .. figure:: _static/motor_fix_bottom.jpg
     :width: 800
     :alt: alternate text
     :align: center
   
     Motor bottom fixation

  .. figure:: _static/motor_cad.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Motor fixed to Motor top fixation

  .. admonition:: todo

     for completeness add picture of how Motor bottom fixation is used to connect frame to Motor top fixation


* **Leg (x 4)**: used to support the drone while on the ground.

  .. figure:: _static/leg.jpg
     :alt: alternate text
     :align: center
     :scale: 50
     
     Leg

* **Battery case with side (x 2), front (x 1) and core (x 1) piece**: used to attach the battery to the frame.

  .. admonition:: todo

     add pictures of all pieces


  .. figure:: _static/battery_assembly.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Battery case

Step-by-step assembly instructions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this section you will learn how to fix all the components on their (custom-made) piece, assemble everything, and solder/connect every cable/connector to electronic components.

**Goal**: assemble the UAV that looks like in this CAD: 

.. figure:: _static/drone.jpg
   :width: 800
   :alt: alternate text
   :align: center
   
   Fully assembled CAD of the T650 UAV

.. admonition:: todo

   in the picture above are missing several components. There is no NUC, battery, Reach M2, Antenna, GPS, ... A final assembly has ALL componenents (even screw). The only thing you cannot draw in CAD are the cables and connectors. If a user want to build it, it helps to see how all componenets fit in the custom-made parts.

**Required tools**:

.. admonition:: todo

   list the tools needed during the assembly: e.g. soldering iron, screwdriver numbers...

**Tips**:

.. admonition:: note

   Every time you solder cables, put a piece of shrink tube beforehand on the cables and heat them on the soldering once it is done.

**Steps**

.. admonition:: todo

   When building the second drone you need to add the steps of "Connecting the NUC on the drone" in this section in a logical order. 
   This should not be a seperate section as you need to explain the build procedure of how to build the autonomous drone (as you design is made for that one).

1. Mount the frame as explained `in this tutorial <https://www.youtube.com/watch?v=Ddvgs200OaY&ab_channel=MultiCopterBuild>`__. You only need to attach the arms to the body and you can skip the assembly of the legs and the top part.

2. Drill the holes of all the 3D printed pieces to deal with imperfections due to shrinkage. Use a drill bit of size 2.5mm for every hole **NOT** directly touching the frame. For the holes used to fix the parts on the frame itself, use a 3mm drill bit.

.. admonition:: todo

   I have difficulties understandign what you mean with these 2 categories of parts. Can you give a list of parts (see names above) that need which drill size? 

  .. figure:: _static/material_motor.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Material for steps 3, 4 and 6 to 9

3. Put the motor on the "Motor top fixation" piece (cables go through the elliptical hole), with the help of the screws provided with the motor. Repeat for all motors.

  .. figure:: _static/motor_top.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Step 3

.. admonition:: todo

   Take picture on white background (use a clean white sheet of paper, you can find those at the printer up the stairs in front of you). Make sure there are no unnecessary things (noise) in the picture. In this case we do not need to see the soldering yet. Crop the image to what is actually useful. **these comments applies to all pictures below you take of the prototype**

4. Solder the three motor cables to the ESC in arbitrary order. Repeat for all ESCs.
(updated)

  .. figure:: _static/step4.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Step 4 
     

  .. admonition:: todo

     Take picture

  .. admonition:: note

     The ordering of the connectors will be corrected later in the calibration phase. Make sure you allow the space to resolder thse cables easily.

5. Solder the three signal cables from the ESC (blue, brown and orange) to a jumper cable. Try to keep the same colors (blue on blue etc). Repeat for all ESCs.

  .. admonition:: todo

     Take picture

6. Fix the "Motor bottom fixation" piece to the frame's part (shown in next figure) with 4x M3 bolts (head on bottom).

(updated)
  .. figure:: _static/step6.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Step 6

7. Put two straps in it through the side windows. The loops will be done downward.

  .. admonition:: todo

     Take picture

  .. admonition:: todo

(updated)
  .. figure:: _static/step6.jpg
     :width: 800
     :alt: alternate text
     :align: center

      Step 6

8. Fix the assembly to the end of an arm, using the bolts and parts (orange and blue) provided with the frame. 

  .. figure:: _static/motor_bottom.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption


  .. admonition:: todo

     orange and blue parts are not clear on the picture   

9. Fix the "Motor top fixation" on the "Motor bottom fixation" with the help of 4x M2.5 bolts (holes on the corner of the parts). Repeat the last four steps for each motor.

  .. figure:: _static/drone_arm_build.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo caption

10. You could now attach the propellors to the motors. However, for safety you should only do this when preparing for a real flight.

  .. admonition:: todo

     Take picture

11. Take four pairs of power supply cables (more or less 18cm long, thick red and black cables, a single pair per motor). Make sure that the four pairs can reach the ESCs starting from the middle of the frame. Solder all the pairs on the power distribution board (PDB). The position of each pair is shown in the picture below. As the UAV requires four motors but the PDB can supply up to eight motors, you can solder each red cable to both B+ connections available on each corner of the PDB. In each corner, where the red cables are, choose one of the two GND connection to solder the black cables.

(updated)
  .. figure:: _static/material_step11.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Material for step 11


  .. figure:: _static/pdb_indications.jpg
     :width: 800
     :alt: alternate text
     :align: center

      Step 11
   
  .. admonition:: todo
      
      The picture above doesn't replace this one, I still need to improve it.
     

  .. admonition:: todo

     Take new picture(s) with the following corrections:
     i) less noise, so do not show jumber cables going to the PDB as you did not tell this before in the instructions. If you forgot this, then add it in a step before. 
     ii) The lengths of the cables: say how long you cut the cables in the text above (e.g. XX cm), 
     iii) a pair with an XT60 connector on them (provided with the PDB) is not visible on the picture what you do with it. So maybe not important here, but say the elngth of the cable so if we don't see the cable we know at elast how long it should be.

12. Fix the PDB to the "Main piece" by help of 4x M3 bolts (head on bottom), use the 4 holes in the middle of the "Main piece".

  .. admonition:: todo

     Take picture

13. Put the free end of each of the ten power supply cables outside the "main piece" with help of the windows of the piece.

  .. admonition:: todo

     Take picture

14. Fix the "Main piece" on the upper plate of the frame, by help of 8x M3 bolts.

  .. admonition:: todo

     Take picture

15. Connect the signal cables of the ESCs (by passing them through the windows of the "main piece") to the "FMU-PWM-out" port of the PDB. Use the pins labeled 1 to 4 (to know which motor to connect to which set of pins, please refer to the chapter "Setting up QGroundControl"). If you have matched rightly the colors of the cables previously, connect the blue ones to the "S" pins, the brown ones to the "+" pins and the orange ones to the "-" pins (on top the blue cables, in the middle the brown cables and at the bottom the orange ones).

  .. admonition:: todo

     Take picture

  .. admonition:: todo

     Chapter Setting up QGroundControl: try to be more specific and say which subsection or image you can find this information.


16. Connect the cables provided with the Pixhawk to the ports "FMU-PWM-in", "PWR1" and "PWR2" of the PDB.

  .. figure:: _static/pdb_connection.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption

  .. admonition:: todo

     be more specific about which cables and how you attach them.

17. Put the Pixhawk in its case and connect these cables respectively to the ports "I/O PWM OUT", "POWER1" and "POWER2" of the Pixhawk.

  .. figure:: _static/PX_pdb_connection.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption

  .. admonition:: todo

     redo and crop figure

18.  Put a cable provided with the Pixhawk on its "DSM/SBUS RC" port. It will be used for the RC receiver.

  .. admonition:: todo

     Take picture

19. Connect the GPS to the Pixhawk using the "GPS MODULE" port.

  .. admonition:: todo

     Take picture

20. Be aware that you'll need to make another connection later. You can do it now but you'll need to follow the steps to make the picoblade cable with jumper wires explained in chapter "Connecting the NUC to the drone".

  .. admonition:: todo

     Take picture

21. You will also need an USB cable to setting up QGroundControl later on, if you want, you can already put the cable on the side of the Pixhawk (and let it hang by a window of the "main piece").

  .. admonition:: todo

     Take picture

22. Fix the Pixhawk case to the "main piece" by help of 4x M2.5 bolts, on the middle stage. Try to have the Pixhawk as horizontal as possible in the drone.

  .. admonition:: todo

     Take picture

23. Solder the battery cables coming from the PDB to each pair coming from the ESCs (black on black, red on red). Don't forget to put beforhand a piece of shrink tube on the cables.

  .. admonition:: todo

     redo picture

  .. figure:: _static/all_untill_optima.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption

24. On top of the "main piece", fix the NUC case by help of 4x M2.5 bolts.

  .. admonition:: todo

     Take picture

25. Put the GPS, the RTK antenna (not yet done) and the Optima (RC receiver) in their respectives cases in the "upper case".

  .. admonition:: todo

     Take picture

26. Fix the "upper case" to the cover of the NUC case, by help of 3x M2.5 bolts.

  .. admonition:: todo

     redo picture

  .. figure:: _static/upper_case_fixed.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption

27. Fix the NUC case cover on top of the NUC case. 

  .. admonition:: todo

     Take picture

28. Use the straps on the motor fixation parts to fix the legs on each arm. Pass the straps through the rectangular holes on the legs and tighten well.

  .. admonition:: todo

     Take picture

29. Assemble the battery case by assembling the sides to the main part of the case. (I don't remeber exactly how many bolts are used with the latest changes, need to check). No need to add the front part to it for now.

  .. admonition:: todo

     Take picture

30. Fix the battery case to the bottom plate of the frame (clear picture with the recent changes to add).

  .. admonition:: todo

     Take picture

31. When needed, put the battery in its case (wires facing the wires hanging from the PDB) and add its front part to disable the movements of the battery.

  .. admonition:: todo

     Take picture

32. With help of tape, fix the ESCs and their wires to the frame such that none of them are hanging.

  .. admonition:: todo

     redo picture

  .. figure:: _static/drone_complete.jpg
     :width: 800
     :alt: alternate text
     :align: center

     todo: caption

(Need to add a picture with the battery case).

Your UAV is built!


Cable-Suspended Payload Module
-------------------------------

.. admonition:: todo

   Raphael: write this section on the the hardware design and construction of the module similar as we explained it for the UAV above. Make sure to take clean pictures on a white background without noise and that they are cropped. (see my comments before for UAV)

This module is installed at the bottom of the UAV and allows to measure the state (position and velocity) of a cable-suspended load hanging below the UAV. 

.. admonition:: todo

   Raphael todo: integrate the next section better in the hardware building chapter using a similar structure as for UAV (see example given below). Give more pictures and explain better each step of the setup.

18.1 Encoders and material needed (TO INTEGRATE BETTER)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In order to measure the position of the load attached to the UAV, we use encoders to measure the angles of ball joint. Based on this information, the position of the payload in the inertial and word frame can be computed easilly (with the other states measured elsewhere of course).
The encoders are the `EMS22A <https://www.bourns.com/docs/product-datasheets/EMS22A.pdf>`__ and their data is read using 
an `Arduino Uno <https://benl.rs-online.com/web/p/arduino/7697409?cm_mmc=BE-PLA-DS3A-_-google-_-PLA_BE_NL_Raspberry_Pi_%26_Arduino_%26_Development_Tools_Whoop-_-(BE:Whoop!)+Arduino-_-7697409&matchtype=&pla-341920527054&gclid=Cj0KCQjwgYSTBhDKARIsAB8KukvAlQU51p7JJ5_edjdlsALUf8YW28bD243x1uw75FKns0QKy6QeSckaAlJREALw_wcB&gclsrc=aw.ds>`__
These encoders are mounted in some spherical joint, 

.. admonition:: todo

   Raphael todo: ADD LINK TO CAD ONCE UPLOADED ON DRIVE/GITHUB . (not done yet as some modifications are possible in case issues are noticed during tests)

On the following figure, one can see the correct circuit to reproduce. 

.. figure:: _static/ElectronicCircuit.png
   :width: 800
   :alt: alternate text
   :align: center

.. note::
  It is better to use flexible cables to do the circuit as rigid ones might disconnect more easily in case they are pulled a bit.

Components
^^^^^^^^^^^
This section lists all the components required to build the Suspended Payload Module for a UAV.
This is currently only compatible with the 650 UAV.

Off-the-shelf
**************

* **Part1**: explain generally.

  *Choice*: specific product name you ordered.

  .. admonition:: todo

     make sure the picture gets cropped (as to delete the excessive white borders)

  .. figure:: _static/frame.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Caption title

  .. admonition:: note

     add a note if required

* **Part2**: do same here
  do same here

* **Other**: 

   Electrical cables: 
      * 20 x this used for that
      * 10 x this used for that

   Electrical connectors: 
      * 3 x this used for that
      * 5 x this used for that
   
   Mechanical connectors (screw, bolts and nuts): 
      * 3 x this used for that
      * 5 x this used for that
   
   Other?:
      * 3 x this used for that
      * 5 x this used for that


  .. admonition:: todo

     add here all screws, nuts, and their sizes, and other things like tape, straps, jumper cables, soldering iron, cables (which type of cable thickness and flexible), connectors (all yellow connectors or metal connectors to power things) and explain for what these are used.
     Old text of Maxime I placed here: All the holes used to attach something directly on the frame are for M3 bolts and the other holes M2.5 bolts. Use M3x12mm and M2.5x12mm bolts.

Custom-made
************
In this section all custom made parts to build the Suspended Payload Module.

.. admonition:: todo

   Once the final designs are finished of both your thesis, I will need the inventor files and all stl files of the UAV (also for the F450 from which you started). I will put them on a drive that people can download it. We cannot put it on github since too large files. 
   They are accessible here (TO DO).

We 3D print all these pieces with 20% infill:

* **Main piece (x 1)**: used to provide enough space for all the components. The PDB is fixed on its lower stage, the Pixhawk and its middle stage and the Intel NUC on its top stage. 

  .. figure:: _static/pb_stage.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Main piece 

* **Motor top and bottom fixation (x 4 per piece)**: used to attach the motors to the frame and to fix the legs.

  .. figure:: _static/motor_fixation_top.jpg
     :width: 800
     :alt: alternate text
     :align: center
   
     Motor top fixation

  .. figure:: _static/motor_fix_bottom.jpg
     :width: 800
     :alt: alternate text
     :align: center
   
     Motor bottom fixation

  .. figure:: _static/motor_cad.jpg
     :width: 800
     :alt: alternate text
     :align: center

     Motor fixed to Motor top fixation

  .. admonition:: todo

     for completeness add picture of how Motor bottom fixation is used to connect frame to Motor top fixation

* **Battery case with side (x 2), front (x 1) and core (x 1) piece**: used to attach the battery to the frame.

  .. admonition:: todo

     add pictures of all pieces


  .. figure:: _static/battery_assembly.jpg
     :width: 800
     :alt: alternate text
     :align: center
     
     Battery case

Step-by-step assembly instructions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this section you will learn how to fix all the components on their (custom-made) piece, assemble everything, and solder/connect every cable/connector to electronic components.

**Goal**: assemble the Suspended Payload Module that looks like in this CAD: 

.. figure:: _static/drone.jpg
   :width: 800
   :alt: alternate text
   :align: center
   
   Fully assembled CAD of the payload module T650 UAV and when attacked to the T650 UAV

**Required tools**:

.. admonition:: todo

   list the tools needed during the assembly: e.g. soldering iron, screwdriver numbers...

**Tips**:

.. admonition:: note

   Every time you solder cables, put a piece of shrink tube beforehand on the cables and heat them on the soldering once it is done.

**Steps**

1. Mount the frame as explained `in this tutorial <https://www.youtube.com/watch?v=Ddvgs200OaY&ab_channel=MultiCopterBuild>`__. You only need to attach the arms to the body and you can skip the assembly of the legs and the top part.

2. Drill the holes of all the 3D printed pieces. Use a drill bit of size 2.5mm for every hole **NOT** directly touching the frame. For the holes used to fix the parts on the frame itself, use a 3mm drill bit.

.. admonition:: todo

   I have difficulties understandign what you mean with these 2 categories of parts. Can you give a list of parts (see names above) that need which drill size? 

.. admonition:: todo

   I assume you drill because they did not print perfectly? Just to be sure you printed the holes with the 3D printer?

Your Suspended Payload Module is built!



  




