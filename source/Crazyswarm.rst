6. Crazyswarm
=============

.. role:: raw-html(raw)
    :format: html

*This chapter will move on to another documentation.*

6.1 Installation
----------------

To install Crazyswarm, follow `these steps <https://crazyswarm.readthedocs.io/en/latest/installation.html>`__. We use Ubuntu 20.04, Pyhton 3.7 and ROS Noetic.
Please refer to the *Physical Robots and Simulation* part. Don't forget to replace what is between brackets in the commands.

To switch between Python2 and Python3 on Ubuntu 20.04, please ckeck `this website <https://www.fosslinux.com/39384/switching-between-python-2-and-3-versions-on-ubuntu-20-04.htm>`__.

At the third step, you might get this error: 

.. code-block:: shell

    E: Unable to locate package gcc-arm-embedded
    
To solve this issue, you juste have to remove ``gcc-arm-embedded`` from the command.

To install the Crazyflie Client, we recommend you to do it with the *Ubuntu Software application*. Then, refer to its `User Guide <https://www.bitcraze.io/documentation/repository/crazyflie-clients-python/master/userguides/userguide_client/>`__.

6.2 Configuration
-----------------

Now, configuration is needed before flying. You should refer to this `user guide <https://www.bitcraze.io/documentation/repository/crazyflie-clients-python/master/userguides/userguide_client/>`__
to `update the firmware <https://www.bitcraze.io/documentation/repository/crazyflie-clients-python/master/userguides/userguide_client/#firmware-upgrade>`__ (it
is also good for you if you read `this <https://crazyswarm.readthedocs.io/en/latest/configuration.html#>`__).

If you can't connect to the CF, you probably need to run this command:

.. code-block:: shell

    cd crazyswarm/crazyflie-lib-python/examples
    python3 write-eeprom.py

You could get this error as a result:

.. code-block:: shell

    Scanning interfaces for Crazyflies...
    Cannot find a Crazyradio Dongle
    Crazyflies found:
    No Crazyflies found, cannot run example
    Traceback (most recent call last):
      File "write-eeprom.py", line 148, in <module>
         while le.is_connected:
    NameError: name 'le' is not defined

If so, set the USB permissions as described `here <https://www.bitcraze.io/documentation/repository/crazyflie-lib-python/master/installation/usb_permissions/>`__.

.. code-block:: shell

    cd crazyswarm/crazyflie-lib-python/examples
    python3 write-eeprom.py
