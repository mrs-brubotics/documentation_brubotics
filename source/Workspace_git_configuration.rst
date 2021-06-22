.. _4. Workspace Git configuration:

4. Workspace Git configuration
==============================

.. role:: raw-html(raw)
    :format: html

In this chapter we will let you know some important things about our Github.

To install our software from our github, please refer to the `README.md <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/README.md>`__ file
available in our github. :raw-html:`<font color="Tomato">[in the next chapter i had the problem that the sourcing line: source GIT_PATH/droneswarm_brubotics/shell_additions/shell_additions.sh
was not automatically added so chapter 8 could not be tested. Check if it is the case for you if not add this line manually to your bashrc file]BC</font>`

These commands will install ``droneswarm_brubotics`` into your work space and install the associated ROS packages.

We always try to organize our `MRS Brubotics github <https://github.com/mrs-brubotics>`__ like the `CTU MRS github <https://github.com/ctu-mrs>`__. 
We have a main github folder droneswarm_brubotics which contains the gitman configurations. There we can link our ROS packages:

* `controllers_brubotics <https://github.com/mrs-brubotics/controllers_brubotics>`__: contains customized controller definitions from Brubotics.

* `trackers_brubotics <https://github.com/mrs-brubotics/trackers_brubotics>`__: contains customized tracker definitions from Brubotics.

* `testing_brubotics <https://github.com/mrs-brubotics/testing_brubotics>`__: contains simulation tmux scripts, custom configs and customized worlds

* :raw-html:`<font color="Tomato">[Please update this list when a new ros package is added to github]BC</font>`

There are also other repositories in our github:

* `MatlabGraphs <https://github.com/mrs-brubotics/MatlabGraphs>`__: to plot graphs from simulations. This folder contains matlab files to allow a new user to follow
  Chapter ?. You can find it indroneswarm_brubotics/useful_files.

* :raw-html:`<font color="Tomato">[not updated]BC</font>` `Crazyflie model Brubotics <https://github.com/mrs-brubotics/crazyflie_model_brubotics>`__, which contains 3D
  models we will implement not done yet :raw-html:`<font color="Tomato">[you can start with this only after D-ERG works on f450 swarm.]BC</font>` :raw-html:`<font color="Tomato">[this
  will be a task for new students.]BC</font>` :raw-html:`<font color="Tomato">[xwewill need the model of the new drone, will be tsk for Zakaria and Frank]BC</font>`
  :raw-html:`<font color="Tomato">[Same for tethered collaborative drones]BC</font>`

* :raw-html:`<font color="Tomato">[Please update this list when a new github folder is added to github]BC</font>`

To add a new ROS package into droneswarm_brubotics you have to update the `gitman.yml <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/.gitman.yml>`__
as it is explained in Section ??, except here we use ssh links and not https. To update the different files you have several options depending on what you want to do:

* You want to update all the code from mrs_workspace and from workspace. Use `pull_all.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/pull_all.sh>`__
  on ``droneswarm_brubotics/script``. [Recommended]

* You want to update only mrs_workspace refer yourself to ??.

* You want to update only brubotics code, go to droneswarm_brubotics and execute : :: 

	git pull 
	gitman install

* You want to update specific folder use in this folder : ::
    
	git pull

I do not figure yet how to update all the submodule by doing a single git pull -which is remap to ``gitman install``- on droneswarm_brubotics, but you can use ``gitman
update``. :raw-html:`<font color="Tomato">[any updates on this?]BC</font>` I think I understand how they do this, in fact on gitman.yaml I refer to other github folder
by targeting master, on mrs, they targeting commit so, they have to always update their gitman.yalm when they push a commit :raw-html:`<font color="Tomato">[i am not
really following what this implies to the solution we have now]BC</font>`
The main point here is that people will just have to install droneswarm_brubotics to get access to our entire project. And with gitman we can work each on a specific
package without affecting each other. 
	
:raw-html:`<font color="Tomato">[make the reader clear when it needs to change stuff in the mrs_workspace and when in the workspace folder. Try to minimize the changes
required in the workspace folder.]BC</font>` Yes, now we are able to add new content in our workspace without making any changes in the mrs_workpsace :raw-html:`<font color="Tomato">[is
this still true?]BC</font>` unfortunately no, we have to modify mrs files to disable all safety features.

If you want to restore mrs code as it was at the beginning, without brubotics code you can use `script/restore_mrs_basic_installation.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/restore_mrs_basic_installation.sh>`__.
Another option is to reinstall everything from scratch [recommended].

In ``droneswarm_brubotics/shell_additions``, you will find `shell_additions.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/shell_additions/shell_additions.sh>`__,
which allows you to use the ``waitBeforeGoTofunction``. This is a bash function which we use in our `session.yaml <https://github.com/mrs-brubotics/testing_brubotics/blob/3f2411da291461cd5396b1925a7c3fd28840176f/tmux_scripts/one_drone_graphs/session.yml#L18>`__
to get automated movement of the drone in our simulation.

.. important::
	Finally in ``script``, you can find `overwrite_mrs_files.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/overwrite_mrs_files.sh>`__ and
	`restore_mrs_files.sh <https://github.com/mrs-brubotics/droneswarm_brubotics/blob/master/script/restore_mrs_files.sh>`__ which are use to disable safety features, as
	explained in Section ??.

:raw-html:`<font color="Tomato">[What about the changes you have to make in the control managers package to allow to use our controllers? Is there a part of code to be
manually added/changed to the uav mangers of CTU each time you start from scratch. If you pull they manger code, is the added lines still there?]BC</font>`
:raw-html:`<font color="Tomato">[write here part that if installation does not work make sure you have SSH key setup, see section 3.1. needed to do this on lenovo and
nuc. See picture error nuc1. also explain tip how to show hidden fies to fidn ssh key manually]BC</font>`
:raw-html:`<font color="Tomato">[i replaced part of interns here. check what to do with it:]BC</font>` The interns packages are link to this folder which contains all
the work of the Brubotics interns. We automated the following steps with the **pull_all.sh** and **install.sh** commands when we update the **droneswarm_brubotics**
folder.

:raw-html:`<font color="Tomato">[explain here shortly how to stage,commit and push]BC</font>`

:raw-html:`<font color="Tomato">[TODO: everyone needs to watch (click watch buttom) the packages of MRS controllers, managers, trackers to make sure if something is changed, we do it too]BC</font>`

:raw-html:`<font color="Tomato">[why we did NOT fork? because it requires repo to be public So one option would be to use a fork explained</font>` `here <https://docs.github.com/en/github/getting-started-with-github/fork-a-repo>`__
:raw-html:`<font color="Tomato">. It is unclear to me if github still requires both repos (so CTU and brubotics) to be public (since we want to keep our private). But i
found also</font>` `this link <https://gist.github.com/0xjac/85097472043b697ab57ba1b1c7530274>`__ :raw-html:`<font color="Tomato">where it seems to be possible for
private repo too. Read all comments since it might be even possible directly from github today. This command is applicable all interns, so  please all read it and
discuss in group. I think this is really important to check in more detail. Please update me after your discussion, then we can decide what's best to do.]BC</font>`
