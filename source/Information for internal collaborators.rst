.. _?. Information for Internal Collaborators:

?. Information for Internal Collaborators
==========================================

?.1 Introduction
-----------------
Welcome internal collaborators (i.e. internal or visiting researchers, MA students)!

Before you can do research efficiently, and demonstrate your ideas on real robots, you need to acquire quite some skills. 
We encourage all beginners to first go through the tutorials listed below as to build sufficient expertise with this framework and all the related concepts and tools.

?.2 Learning the Skills
-----------------------
It is advised to learn the skills step by step in the order as listed below. 
Please contact your supervisor if you have questions (e.g. which points you could possibly skip or if things don't work as expected from the tutorial).

?.2.1 Communication
-------------------
A good communication between the student and the supervisor is key to achieve good research results on time.

Some guidelines:
    * Update your supervisor every few days you worked on some part. You should refer him/her to the tutorial documentation and to the scientific research report. Explain the new ideas you have, the issues you solved, the open problems you still have, what you already tried to solve these without success, and what you will try next (hopefully with sucess).
    * Once a week we will try to organize a microsoft teams or a physical meeting (about 30 minutes) where you can provide updates of the last week, and a plan of what you will work on in the coming week. A powerpoint is the best medium, but you can also show parts of your report.
    * Make realsitic plannings and don't delay working on it. It requires you to spend quite some focused time to debug issues and obtain good results.
    * Use email with references to the tutorial and scientific report if you want detailed technical feedback.
    * If you arrange the meeting date and hour on time, the more chance your supervisor will be free.
    * Use WhatssApp message for urgent communication (e.g. the code does not build today, are you in the lab, where can I find this in the lab, ...). Send your supervisor your phone number, who will invite you to the whatssapp group. It is adviced to use the whatssApp web app on your PC to type quicker (highly advised to not lose time during Q&A).
    * If you and your supervisor don't know the answer to some issue, ask your questions (if related) on the issues (e.g. github issues) of the framework you are using. Formulate these questions clearly, but don't make them too long. Outside collaborators might have no clue (as oppsoded to yourself and your supervisor) what you are trying to do.

?.2.2 The Tutorial Documentation
--------------------------------
We ask all collaborators to document their research (also what does NOT work is valuable!) in easy-to-read and well-explained tutorials.
Moreover we ask them to improve the unclear or wrong parts of the existing tutorials while they go through them.

A good documentation:
    * is a tutorial-style report which allows seemless reproducability of research results;
    * automatically teaches the new collaborator and does not require additional help from an expert to reproduce the results;
    * does NOT look like a scientific paper or a thesis report. The focus of a tutorial is on the pratical setup and implementation aspects to reproduce experiments and not on the analysis of the research results;
    * is obtained when you document directly after you try new things. Delaying the writing for the next hour, the day or the next week will most likely reduce the clarity and cause pontial errors like: missing important details, wrong orders of steps. It quickly leads to non-reproducible results and no help from other collaborators.
    * evolves over time and is never finished since it can always be improved.
Remember, the better you document your work in this tutorial, the easier you will get help from others and the more chance your high-quality research will be used in the end!

Take a look at the files in this repository. If you understand the syntax of the .rst files and the structure you are ready to contribute your own documentation to this Read the Docs tutorial. Ask your advisor where you should put the documentation of your project.

Note: Most of the documentation (this repo) and all code can be found on github. Also provide your gmail account to your advisor and request access to the google drive since some information is only available there.
    
?.2.3 The Scientific Research Report
------------------------------------
Everything related to the actual scientific research questions, the followed research methodology, the research results, discussions and conclusions will be contained in the scientific research report (e.g. the paper, the thesis).
    * Create your scientific report in the requested template. Please contact your supervisor since they might have example templates to start from.
    * Preferably use the online overleaf Latex editor https://www.overleaf.com/ and share the report with your supervisor.
    * Go to the main.tex and ctrl + f "NewStudent". Copy paste and update the example code in there such that you and your supervisor can give comments (i.e. coloured text) using their own initials (e.g. \bc{Bryan Convens (BC) writes a comment here!}). Don't delete the example NS \ns{it is for new students to see this example.
    
Some guidelines:
    * Before you write a part, create a structure for the report with sections, subsections, subsubsections, and explain very briefly the main message you want to bring to the reader in each of these.
    * Write in decent English and with full sentences.
    * Use SI units and their abreviations.
    * Think well about the used notations and symbols. Try to use the same notations and symbols as the work where you base yourself on. It will help you to make less mistakes.
    * The results section needs high-quality figures and tables. This means the axes should be readable, the graph lines should be thick enough and identifyable with a legend, and the caption should provide enough information to the reader to understand the result (e.g. captions which are only a title are mostly bad). 
    * Be critical. Question yourself if the results you obtained are plausible and if not what might be wrong.
    
?.2.4 Setting Up your Ubutnu Machine
------------------------------------
Working on advanced robot simulators (e.g. Gazebo) and algorithms requires decent computational hardware (i.e. CPU, GPU specs).
We have several computers available in the lab, but these have to be reserved since they are mainly used when working on the real robot hardware.

Please provide your supervisor the specs of your machine(s) (i.e. laptops or desktops) you would like to use for this project. They can tell you if it's sufficient.
Once you decided on a machine:
    * TODO FROM PART INTERNS You first need to install Ubuntu 18.04 LTS Desktop. If you already have windows on your PC, you need to reserve HDD space (a partition of at least 50GB recommended) and do a dual boot. Do NOT use a virtual machine, it slows down things a lot. Follow \href{https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview}{these steps}. It is advised to boot from a usb stick.
    * Configure the VUBnext internet settings on Ubuntu. In the Security tab select:
         * Security: WPA & WPA2 Enterprise
         * Authentication: Protected EAP (PEAP)
         * CA certificate: (None)
         * Check the "No CA certificate is required box.
         * PEAP version: Automatic
         * Inner authentication: MSCHAPv2
         * Fill in your VUB username and password.
    * Download the \href{https://code.visualstudio.com/}{visual studio code IDE} for Ubuntu and install it. Preferably use this whenever you want to view or edit code opposed to the default text editor in Ubuntu. Set visual studio code as the default program to open files (right click on the file and select "open with other application").
    * Read and follow step doc in google drive: Configure CPU Specs. Laptops only have performance and powersave mode and no Conservative, ondemand and schedutil mode. Make sure you do your simulatios always in performance mode.
    * Install \href{https://www.teamviewer.com/nl/download/linux/}{teamviewer for linux}, and create an teamviewer account. 
    * Install on Matlab and Simulink version 2020b. See doc in google drive, since you might get some non trivial issues.

?.2.5 Git Version Control
-------------------------

    * Create a github account and email me your name on github. I will give you access to our code.
    * Learn git by following \href{https://www.coursera.org/learn/version-control-with-git}{this free tutorial}. Make sure you follow the tutorial from the command line / terminal window (not the GUI). This will allow you to effectively improve your software and work in a team. 
    * You will further use git during the project. Remember to keep your commits structured by having multiple commits for each small task you code. Try to push your code on github once a day so everyone is up-to-date with your developments.
    * Test if your code works before commiting anything!

?.2.6 C++ Software Development
------------------------------
Follow this \href{https://www.youtube.com/watch?v=vLnPwxZdW4Y}{quick C++ tutorial for beginners}. No need to do things, just follow it.


?.2.7 ROS Software Development
------------------------------
Learn the basics and intermediate ROS concepts and tools by reading and testing the examples in the \emph{Mastering ROS for Robotics Programming} and its related github which can be found in our google drive. Read the following chapters in this book: ch1, ch2, ch3, ch4, NOT ch5, ch6, ch7, ch8 (nodelets very important), ch15. Although the books is written for ROS kinetic, just use ROS melodic on Ubuntu 18.
 
?.2.8 The CTU MRS Framework
---------------------------
TODO REFER TO SECTION ON THIS!

The software framework you will use during the project is based on \href{https://ctu-mrs.github.io/}{the MRS UAV system code from CTU Prague}. 
    * Read their wiki \href{https://ctu-mrs.github.io/} for the parts that are relevant for your thesis and install their code" mrs uav system" by following the steps found \href{https://github.com/ctu-mrs/mrs_uav_system#i-have-a-fresh-ubuntu-1804-and-want-it-quick-and-easy}{on their github repo}: "I have a fresh Ubuntu 18.04 and want it quick and easy". Than try to compile (i.e. build) the code by following these steps. You should NOT istall their linux setup. 
    * Learn the required skills from the links they provide.
    * In case you have problems only related to this software, please open a new issue \href{https://github.com/ctu-mrs/mrs_uav_system/issues}{here} or open a new discussion. Validate if the software builds without errors and without warnings. 
    * Run a some example scripts in the simulationfolder. Which ones do (not) work?
    * Read the paper of mrs uav system https://link.springer.com/article/10.1007/s10846-021-01383-5

?.2.9 Our droneswarm_brubotics Framework
----------------------------------------
Read the relevant parts of our tutorial to learn to use the droneswarm_brubotics framework.
Please help us to improve the tutorual. If you struggled on some parts it means it was not writtin sufficiently well. 
Don't forget to commit your changes when updates this tutorial!





