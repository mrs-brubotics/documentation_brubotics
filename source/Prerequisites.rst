Prerequisites
====================
    
This section gives an overview of the main scientific concepts and engineering tools used this project.
We suggest some useful sources for both beginners (more general) and for more advanced audience (more sepcific).
Depending on the features you are interested in, not all prior knowledge will be required.

.. admonition:: todo

   It is highly recommended to read through the Introduction section of `the CTU MRS tutorial <https://ctu-mrs.github.io/docs/introduction/>`__.
   as this project is heavily based and can generally be considered as an extension on the former. 
   We even recommend to checkout the full CTU MRS tutorial to get a glimpse fo its capabilities.

Scientific disciplines
------------------------

.. admonition:: todo

   list some relevant books/papers to be read.


* Dyanmical Systems Theory
* Automatic Control Theory
* Robotics and Automation 
* Safety Verification


Software development
-----------------------------

Ubuntu
^^^^^^^
 Ubuntu 20.04 with at least 50GB disk space. Better to use a good desktop machine for simulation. need/ tested with ROS melodic and matlab simulink 2020a.

PC Specs
*************

Before dealing with the Ubuntu partition, we have to talk about your minimum PC configuration. Here you can see the general PC specs used by the four interns
during the project:

.. figure:: _static/Intern_PC_specs.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 2.1: Intern PC specs

However, this configuration above could not be enough when we have to launch simulations that need a lot of resources from our PC. We may have issues with the
spawn and takeoff of the UAV later. That is why we rather recommend this kind of PC specs:

.. figure:: _static/Bryan_PC_specs.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 2.2: Bryan Convens PC specs

This type of configuration is better for simulation launch. We rather recommend a PC with a processor in this category. Indeed, the CPU performance will have a
significant impact on the build of the packages in your workspace, and the proper launch of your simulation when you will need to launch more than three UAV.

.. note::
   Keep in mind that some issues with your simulation will not be related to your PC configuration. We had issues with the launch of many UAV, due to the lack
   of code optimization.

Ubuntu partition
*******************

Then, we can talk about the Ubuntu partition. You will need a minimum **50GB** partition on your PC to install this partition.

.. note::
   The Ubuntu 18.04 version will not be updated in the future. That is why the ctu-mrs team is adapting their work for the 20.04 version (more detail
   `here <https://github.com/ctu-mrs/mrs_uav_system/issues/9>`__). However, it is still possible today to work with the 18.04 version without any issue.

That is why we recommend to install the Ubuntu 18.04 version until the change on 20.04 have been made.

Prepare Ubuntu
*****************

:red:`[Matlab]BC` :red:`[Micorosdt visual studio code editor set as default]BC`

To install Ubuntu, you will first need to get a bootable USB stick [Recommended] or a dvd. 

To create the bootable usb disk you can follow `these steps <https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#3-usb-selection>`__
if you are running on Windows and `these <https://ubuntu.com/tutorials/create-a-usb-stick-on-ubuntu#5-confirm-usb-device>`__ if you are running on Ubuntu.

Install Ubuntu
******************

To install this OS, please refer to the the `Ubuntu install documentation <https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview>`__.


Programming
^^^^^^^^^^^^^^

Environemnts, Languages, Concepts

Visual Studio Code
**********************

We highly recommend you to use `Visual Studio Code <https://code.visualstudio.com/>`__ for Ubuntu. It is easier to view and edit code with syntax highlighting
tools. You just need to install extension depending on what kind of file you are working e.g. ``.cpp``, ``.py`` etc. Set visual studio code as the default
program to open files (right-click on the file and select "Properties → Open With → Visual Studio Code → Set as default").

To add line comments, select the lines with right-click and use ``Ctrl+K Ctrl+C``.
To remove them, use ``Ctrl+K Ctrl+U``.

C++
****

Python
*******

ROS
****
MRS bases its work on the use of ROS, a framework widely used in robotics. We strongly recommend that you inquire about. You can find useful tutorial on `ROS wiki <http://wiki.ros.org/>`__.
We also recommend `Mastering ROS for ROBOTICS Programming <https://drive.google.com/file/d/1URHQAuDK1JT6X-8AUVfGe2xsPoZxqJsY/view>`__, by Lentin Joseph ans Jonathan Cacace, chapter 1, 2, 3, 4 and 7 - `here <https://github.com/jocacace>`__ you will find the github
from Jonathan Cacace.

Matlab
********


Git Version Control
**********************

**Git basics**

If you are new at git, first take a look at `basic commands <https://guides.github.com/introduction/git-handbook/#basic-git>`__. We can base our usage of git on
the team on this `tutorial <https://learngitbranching.js.org>`_ or the `git tutorial advised by Bryan <https://www.coursera.org/learn/version-control-with-git>`__.
Here are some useful commands:

.. code-block:: shell

   git branch <branchname> 

Creates a new working branch independent of the master. Before committing, make sure you are on your branch and not in the master
with ``git checkout <branchname>``. You can also use ``git -b checkout <branchname>`` in order to create new branch and directly work on it.

.. code-block:: shell

   git merge <branchname>

Merges two branches, preserving the barnch structure. Be careful when merging branches: even though Git has mechanisms to make merging
seamless and simple, it can result in loss of important parts of the code. Some text editors, like `Atom <https://atom.io/>`__ and `VSCode <https://code.visualstudio.com/>`__
provide useful tools to work with git and help prevent loss of information on merges.

.. code-block:: shell

   git rebase <branchname>

Combines two branches and **deletes** one of them, usually the one that is not the master. All the commit history is transferred to the other branch, and in the
timeline of the repository, the other branch is not preserved.

.. code-block:: shell

   git cherry-pick <commitID1> <commitID2> ...

Applies the selected commit from one branch to another.

.. code-block:: shell

   git rebase -i HEAD~<numberofselectedcommit>
   
Similar to cherry-pick, it allows the reorganization of commits from the current branch.

.. code-block:: shell

   git stash

*Reverts the folder to the latest commit and throws all changes away*

**Setup your SSH key in Git**


We recommend you to setup your SSH key in Github, it's an easy thing that can avoid you some problems that are hard to understand.
Also, you will need to setup your email and your name by using these commands:

.. code-block:: shell

   git config --global user.name "FIRST_NAME LAST_NAME"
   git config --global user.email "MY_NAME@example.com"

To verify your configuration file, you can run the same commands but like this:

.. code-block:: shell

   git config --global user.name
   git config --global user.email

Now, you can follow `these steps <https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key>`__
to setup your SSH key in Github. 
Note: when this message is given "Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/nuc3/.ssh/id_ed25519):", you can either keep the default or set .ssh/FirstName-LastName. With the latter we can keep track which ssh keys belong to which github account.

**How to use github permalinks**


A Github permalink is an interesting way to refer to some lines of a code, here is how to create one. For example in the file `control_manager.cpp <https://github.com/mrs-brubotics/
MatlabGraphs/blob/master/control_manager.cpp>`__, if you want to highlight the line 5, you need to click on the 5 line number:

.. figure:: _static/canvas1.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 2.3: Generation of a link to highlight the line 5

You can see in the link at the top that the 5 line is highlighted in the permalink. If you want to highlight from the line 5 to the line 7, you need to hold the
"MAJ" key and to click on the line 7:

.. figure:: _static/canvas2.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 2.4: Generation of a link to highlight the lines from 5 to 7

You can see in the link at the top that the lines from 5 to 7 are highlighted in the permalink. With this link, if the file is updated, you may refer to lines
which are not the desired ones. In order to freeze the file as it is when you create the permalink, you need to press the "y" key after the lines are highlighted:

.. figure:: _static/canvas3.png
   :width: 800
   :alt: alternate text
   :align: center

   Figure 2.5: Modification to the link to always highlight the intended lines

You can see that the link at the top changed to always refer to this version of the file.

:red:`[The idea of this report is to provide a detailed tutorial on how the use and understand the MRS software and how this is extended for
research on control and navigation of aerial robots.]BC` :red:`[From this report you create something similar as the ROS book, but for a
very specific project.]BC` :red:`[this report is explained for newcomers in robot software development.]BC`
:red:`[explain the outline of the report. what will be discussed in each chapter.]BC`

CTU MRS UAV System
*********************
In this documentation, you will do the simulation of one drone with gps. Remember that you can use the :ref:`Tmux commands <2.5_Working_with_Tmux_session>`
to navigate through different terminals.

Also, you can try to launch others simulations created by CTU-MRS group. Go to the ``~/mrs_workspace/src/simulation/example_tmux_scripts/`` folder and
run the ``ls`` command to see the name of the different simulations. Then you can run:

.. code-block:: shell

   cd "DESIRED SIMULATION"
   ./start.sh

It will launch a Tmux session and Gazebo. It depends on your PC specs but you may not be able to run all the simulation, especially multi-drones ones.

Tmux sessions
*******************

Tmux is a powerful tool with ROS and Linux. It allows you to setup a complete and custom session for your simulation for instance, or been able to custom your
terminal as you want. Your will be able to use this tool if you have install the MRS Linux environment setup as recommented in section :ref:`2.6.1 <2.6.1_MRS_workspace_installation>`.
Multiple commands could be used to navigate in the tmux session. We can find a complete data-sheet of the tmux commands in the `MRS lab ROS platform Cheat Sheet <https://drive.google.com/drive/folders/1mCFhz56bAgA8XrwsXxz6VisY9S4GDYLP>`__
and in the `Ubuntu Tmux documentation <http://manpages.ubuntu.com/manpages/trusty/man1/tmux.1.html>`__. The following tmux commands are the most important ones
to navigate in the tmux session without any issue:

* ``<Ctrl-a and k>``: Killing tmux session (and also the simulation)
* ``<Ctrl-a and n>``: Navigation between tmux windows
* ``<Ctrl-a and number>``: Navigate to the wanted terminal
* ``<Ctrl-a and arrows>``: Navigation between windows in the same terminal
* ``<Ctrl-a and w>``: Listing tmux windows
* ``<Ctrl-a and[>``: Scrolling in the current window
* ``<Ctrl-t>``: New tmux window in the current session

.. note::
   You don't need to press Ctrl-a-k at the same time, you need to press Ctrl-a and then after k or n, etc.

.. note:: 
   The official tmux documentation use ``<Ctrl+b and ['key']>`` format. In our case, ``<Ctrl-a and ['key']>`` is the good way to issue the command. It's also possble
   that you get different shortcut, you can easily remap command into ``~/.tmux.conf``.


.. admonition:: todo
   check summer school shortcuts and add here, check MRS cheatsheet


Hardware development
----------------------

CAD
^^^^^

3D Printing
^^^^^^^^^^^^^


Soldering
^^^^^^^^^^^^^

