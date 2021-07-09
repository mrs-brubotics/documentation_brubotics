6. Crazyswarm
=============

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

6.2 Configuration
-----------------

To install the Crazyflie Client, we recomment you to do it with the *Ubuntu Software application*. Then, refer to its `User Guide <https://www.bitcraze.io/documentation/repository/crazyflie-clients-python/master/userguides/userguide_client/>`__.