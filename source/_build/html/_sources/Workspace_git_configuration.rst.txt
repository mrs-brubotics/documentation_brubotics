.. _4. Workspace Git configuration:

4. Workspace Git configuration
==============================

.. role:: raw-html(raw)
    :format: html

In this chapter we will let you know some important things about our Github.

To install our software from our github, please refer to the `README.md <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/README.md>`__ file
available in our github. These commands will install droneswarm_brubotics into your work space and install the associated ROS packages.

We always try to organize our `MRS Brubotics github <https://github.com/mrs-brubotics>`__ like the `CTU MRS github <https://github.com/ctu-mrs>`__. 
We have a main github folder droneswarm_brubotics which contains the gitman configurations. There we can link our ROS packages:

• `controllers_brubotics <https://github.com/mrs-brubotics/controllers_brubotics>`__: contains customized controller definitions from Brubotics.

• `trackers_brubotics <https://github.com/mrs-brubotics/trackers_brubotics>`__: contains customized tracker definitions from Brubotics.

• `testing_brubotics <https://github.com/mrs-brubotics/testing_brubotics>`__: contains simulation tmux scripts, custom configs and customized worlds

There are also other repositories in our github:

• `MatlabGraphs <https://github.com/mrs-brubotics/MatlabGraphs>`__: to plot graphs from simulations. This folder contains matlab files to allow a new user to follow Chapter ?. You can find it indroneswarm_brubotics/useful_files.

To add a new ROS package into droneswarm_brubotics you have to update the `gitman.yml <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/.gitman.yml>`__ as it isexplained in Section ??, except here we use ssh links and not https.
To update the different files you have several options depending on what you want to do:

• You want to update all the code from mrs_workspace and from workspace.  Use `pull_all.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/pull_all.sh>`__ ondroneswarm_brubotics/script. [Recommended]

• You want to update only mrs_workspace refer yourself to ??.

• You want to update only brubotics code, go to droneswarm_brubotics and execute : :: 

	git pull 
	gitman install

• You want to update specific folder use in this folder : ::
    
	git pull

• If you want to restore mrs code as it was at the beginning, without brubotics code you can use `script/restore_mrs_basic_installation.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/restore_mrs_basic_installation.sh>`__. Another option is to reinstall everything from scratch [recommended].

In ``droneswarm_brubotics/shell_additions``, you will find `shell_additions.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/shell_additions/shell_additions.sh>`__, which allows you to use the ``waitBeforeGoTofunction``. 
This is a bash function which we use in our `session.yaml <https://github.com/mrs-brubotics/testing_brubotics/blob/3f2411da291461cd5396b1925a7c3fd28840176f/tmux_scripts/one_drone_graphs/session.yml#L18>`__ to get automated movement of the drone in our simulation.

.. important::
	Finally in ``script``, you can find `overwrite_mrs_files.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/overwrite_mrs_files.sh>`__ and `restore_mrs_files.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/restore_mrs_files.sh>`__ which are use to disable safety features, as explained in Section ??.

