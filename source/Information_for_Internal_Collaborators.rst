Information for Internal Collaborators
==========================================

Introduction
-----------------
Welcome internal collaborators (i.e. internal or visiting researchers, MA students)!

Before you can do research efficiently, and demonstrate your ideas on real robots, you need to acquire quite some skills. 
We encourage all beginners to first go through the tutorials listed below as to build sufficient expertise with this framework and all the related concepts and tools.

Learning the Skills
-----------------------
It is advised to learn the skills step by step in the order as listed below. 
Please contact your supervisor if you have questions (e.g. which points you could possibly skip or if things don't work as expected from the tutorial).

Communication
^^^^^^^^^^^^^^
A good communication between the student and the supervisor is key to achieve good research results on time.

Some guidelines:
    * Update your supervisor every few days you worked on some part. You should refer him/her to the tutorial documentation and to the scientific research report. Explain the new ideas you have, the issues you solved, the open problems you still have, what you already tried to solve these without success, and what you will try next (hopefully with sucess).
    * Once a week we will try to organize a microsoft teams or a physical meeting (about 30 minutes) where you can provide updates of the last week, and a plan of what you will work on in the coming week. A powerpoint is the best medium, but you can also show parts of your report.
    * Make realsitic plannings and don't delay working on it. It requires you to spend quite some focused time to debug issues and obtain good results.
    * Use email with references to the tutorial and scientific report if you want detailed technical feedback.
    * If you arrange the meeting date and hour on time, the more chance your supervisor will be free.
    * Use WhatssApp message for urgent communication (e.g. the code does not build today, are you in the lab, where can I find this in the lab, ...). Send your supervisor your phone number, who will invite you to the whatssapp group. It is adviced to use the whatssApp web app on your PC to type quicker (highly advised to not lose time during Q&A).
    * If you and your supervisor don't know the answer to some issue, ask your questions (if related) on the issues (e.g. github issues) of the framework you are using. Formulate these questions clearly, but don't make them too long. Outside collaborators might have no clue (as oppsoded to yourself and your supervisor) what you are trying to do.

The Tutorial Documentation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
    
The Scientific Research Report
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Everything related to the actual scientific research questions, the followed research methodology, the research results, discussions and conclusions will be contained in the scientific research report (e.g. the paper, the thesis).
    * Create your scientific report in the requested template. Please contact your supervisor since they might have example templates to start from.
    * Preferably use the `overleaf online Latex editor <https://www.overleaf.com>`__ and share the report with your supervisor.
    * Go to the main.tex and ctrl + f "NewStudent". Copy paste and update the example code in there such that you and your supervisor can give comments (i.e. coloured text) using their own initials (e.g. \bc{Bryan Convens (BC) writes a comment here!}). Don't delete the example NS \ns{it is for new students to see this example.
    
Some guidelines:
    * Before you write a part, create a structure for the report with sections, subsections, subsubsections, and explain very briefly the main message you want to bring to the reader in each of these.
    * Write in decent English and with full sentences.
    * Use SI units and their abreviations.
    * Think well about the used notations and symbols. Try to use the same notations and symbols as the work where you base yourself on. It will help you to make less mistakes.
    * The results section needs high-quality figures and tables. This means the axes should be readable, the graph lines should be thick enough and identifyable with a legend, and the caption should provide enough information to the reader to understand the result (e.g. captions which are only a title are mostly bad). 
    * Be critical. Question yourself if the results you obtained are plausible and if not what might be wrong.
    
Setting Up your Ubutnu Machine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Working on advanced robot simulators (e.g. Gazebo) and algorithms requires decent computational hardware (i.e., CPU, GPU specs).
We have several computers available in the lab, but these have to be reserved since they are mainly used when working on the real robot hardware.
Please provide your supervisor the specs of your machine(s) (i.e., laptops or desktops) you would like to use for this project. They can tell you if it is sufficient.

Once you decided on a machine to use:

   * You first need to install Ubuntu 20.04 LTS Desktop. 
   * If you already have windows on your PC, you need to reserve HDD space (a partition of at least 50GB recommended) and do a dual boot. Do NOT use a virtual machine, it slows down things a lot. 
   * Follow `these steps <https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview>`__.
   * In case you need to update the on-board UAV Intel NUC computer follow `these install instruction <https://ubuntu.com/download/intel-nuc-desktop>`__, make sure you updated the BIOS (follow `these the F7 update instructions <https://www.intel.com/content/dam/support/us/en/documents/mini-pcs/AptioV-BIOS-Update-NUC.pdf>`_ where you choose the BIOS update for Intel® NUC Kits NUC10i7FN `these steps <https://www.intel.com/content/www/us/en/download/19485/bios-update-fncml357.html?wapkw=NUC10FNK%20bios%20update>`_.)
   * Download the Ubuntu Desktop image `here <https://releases.ubuntu.com/20.04/?_ga=2.186935080.1248387199.1654872293-148428191.1654872293&_gac=1.51465691.1654880802.CjwKCAjw14uVBhBEEiwAaufYx895DiQpFQjDlt3YTGCU2WhtA7pSPgYMvkcwDVmSvlFvOo2gUVrLQBoCOP0QAvD_BwE>`_.
   * It is advised to boot from a usb stick. Te format the stick we recommand using SD Card Formatter on windows.
   * When booting the Intel NUC with the Ubuntu USB drive, you have to push F10 to enter the boot menu.
   * In case the install doe not start immediately and you don't see an "install Ubuntu" option, then fix the problem as follows:
      * `Here <https://askubuntu.com/questions/1388118/no-install-ubuntu-option-when-booting-from-live-usb>`__, is mentioned that in the terminal, "I changed "quiet splash" to "noacpi acpi=off" as suggested in the first bullet of the answer linked. Press F10 to boot and it worked." This did not work on the NUC.
      * `Here <https://askubuntu.com/questions/1138820/black-screen-after-grub-selection-boot-from-usb-live> __ they mentioned but "with adding nouveau.modset=0 to the end of the line instead of acpi=off and leaving quiet splash in place". This did work on the NUC. Normally afterwards, the Ubuntu installation is loaded immediately when starting up. 
   * Note that in case you have multiple ubutnu partitions and you wish to delete there is no trivial solution for this. Therefor always install over the previous version.

   *  If you did not select the correct keyboard during the Ubuntu installation, 
      there can be some bugs with a Belgian AZERTY keyboard. 
      Some solutions:

         * Install the Languages French and Dutch (Nederlands).
         * sudo locale-gen fr_BE.UTF-8 (https://askubuntu.com/questions/1133361/cannot-find-my-keyboard-layout)
         * Find Belgian Wang 724 AZERTY under Dutch or French. https://www.roelpeters.be/changing-to-dutch-belgian-keyboard-layout-in-ubuntu/
         * Move it up as the default keyboard.  

    * Configure you internet access:

         * VUBnext internet settings on Ubuntu. In the Security tab select:
               * Security: WPA & WPA2 Enterprise
               * Authentication: Protected EAP (PEAP)
               * CA certificate: (None)
               * Check the "No CA certificate is required box.
               * PEAP version: Automatic
               * Inner authentication: MSCHAPv2
               * Fill in your VUB username and password. 
         * Other option is Eduroam. TODO: Please explain someone who works with Eduroam.
         * In lab there is R&MM network with password "nietholonoom". The range is very limited.
         * Note: if this machine is an onboard computer of a drone, make sure that the device is set to never automatically connect to a network (except the main router's network). So on yuor network Detail, uncheck the box "Connect automatically".
         
    * Regularly update Ubuntu. Do this at least once a week.
    
         .. code-block:: shell
         
                 sudo apt-get update
                 sudo apt-get upgrade 
            
    * Install htop so you can kill processes if required as exaplined here in the last comment (https://askubuntu.com/questions/596830/kill-process-with-htop) F9.
    
          .. code-block:: shell
         
             sudo snap install htop
             
       * First, press F6 which is the "sort by" option. 
       * Then, under the "sort by" category on the left, select option PID and then press Enter. This should give you a more stable output.
       * Next, to locate a process, press F3 to search, type in the search, and then press F3 again to scroll through search results.
       * When the process is highlighted, press F9 two times quickly and then press 9 and then press Enter to kill the process.
       
    * Configure CPU Specs:
         * Read the section `Disabling CPU frequency scaling <https://frankaemika.github.io/docs/troubleshooting.html#disabling-cpu-frequency-scaling>`__  
         * Install cpufrequtils and its indicator
         
            .. code-block:: shell

               sudo apt install cpufrequtils
               sudo apt install indicator-cpufreq     
         
         * Reboot the machine to see the cpu indicator appear in the top right corner of your screen. You can manually select the desired mode here.
         
            .. code-block:: shell
            
               sudo reboot
               
         * Automatically enable the machine in performance mode on every boot (required for the onboard drone computer, and recommended for other machines):
         
            .. code-block:: shell

               sudo systemctl disable ondemand
               sudo systemctl enable cpufrequtils
               sudo sh -c 'echo "GOVERNOR=performance" > /etc/default/cpufrequtils'
               sudo systemctl daemon-reload && sudo systemctl restart cpufrequtils
               
         * Note: laptops only have Performance and Powersave mode and no Conservative, ondemand and schedutil mode. Make sure you do your simulatios always in performance mode. See also .docx on our Google drive.
         * Make a habit to do all simulations and experiments in perfromance mode. This can significantly lower the computational time of simulations and allow to achieve better real-timeness.
    * Download the `Visual Studio Code IDE <https://code.visualstudio.com/>`__ for Ubuntu (.deb) and install it. Preferably use this whenever you want to view or edit code opposed to the default text editor in Ubuntu. Set visual studio code as the default program to open files (right click on the file and select "open with other application").
    * Install `TeamViewer for Linux <https://www.teamviewer.com/nl/download/linux/>`__, and create an teamviewer account. 
    * Install on Matlab and Simulink version 2021b and the toolboxes you like. See doc in google drive, since you might get some non trivial issues.
    * Install RecordMyDesktop from UbuntuSoftware. Use this for recording simulations of Gazebo, RVIZ, since the default recording tools perform poorly.

Git Version Control
^^^^^^^^^^^^^^^^^^^^^^^
    * Create a github account and email me your name on github. I will give you access to our code.
    * Setup git user name and email on your machine by following these steps: https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-18-04 , "Setting Up Git". TODO Give example how .gitconfig should look like.
    * You need to setup your ssh keys correctly by following [these steps](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to generate them and then follow these steps https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account to add them to your GitHub.
    * Also since August 2021 developers are required to use [personel access tokens](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/). Follow [these steps](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) to generate these tokes.
    * Learn git by following \href{https://www.coursera.org/learn/version-control-with-git}{this free tutorial}. Make sure you follow the tutorial from the command line / terminal window (not the GUI). This will allow you to effectively improve your software and work in a team. 
    * You will further use git during the project. Remember to keep your commits structured by having multiple commits for each small task you code. Try to push your code on github once a day so everyone is up-to-date with your developments.
    * Test if your code works before commiting anything!
    * The typical workflow with git is:
        
      .. code:: shell
      
         cd to_a_git_repo 
         git checkout branch_name (e.g. master, main)
         git status
         git pull
         git add file_names (tab tab) or git add -A (for all files)
         git commit -m "write a clear but comprehensive commit message"
         git push origin branch_name (e.g. master, main)
               
    * Learn about managing large files with Git [here and all sublinks](https://docs.github.com/en/github/managing-large-files). If you regularly push large files to GitHub, you should use Git Large File Storage (Git LFS). You can learn the basic use quickly from [this](https://git-lfs.github.com/) and [this](http://arfc.github.io/manual/guides/git-lfs) link and more details including install instructions can be found [here](https://docs.github.com/en/github/managing-large-files/versioning-large-files). [ONLY ALLOWED BY PROJECT OWNERS!!!] For rewriting git history we suggest you to use BFG Repo-Cleaner () over git filter-branch. More info on both you can find here (https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository).
         * For installation: download the latest jar file from https://rtyley.github.io/bfg-repo-cleaner/ and rename it to just bfg.jar. Run sudo apt-get install build-essential procps curl file git (source https://docs.brew.sh/Homebrew-on-Linux) and then try brew install bfg (source https://github.com/rtyley/bfg-repo-cleaner/issues/255#issuecomment-606705860) and it will be built. Now you can run bfg as java -jar /path/to/bfg-version.jar (so you don't have to make the alias) (source: https://github.com/rtyley/bfg-repo-cleaner/pull/196/commits/5f2e8879117da42b71304da5febed93f887e0fd0). 
         * Cleaning the repo (source https://rtyley.github.io/bfg-repo-cleaner/):
               * Commit all files and push them remotely
               * Make a manual copy of the repo you want to clean in case something goes wrong
               * Write down the folder size of the repo and its .git folder (which contains the commit history).
               * Open a new terminal and create a folder to clone the mirror packe in it:
               
                  .. code-block:: shell
                     
                     cd Desktop
                     mkdir folder_name_mirrored_repo
                     cd folder_name_mirrored_repo
                     git clone path_to_remote_repo
                     
               * Move the bfg.jar file to Desktop
               * In a new terminal clone the repo in the folder
                  .. code-block:: shell
                     
                     cd Desktop/folder_name_mirrored_repo
                     git clone --mirror remote_repo_name.git
                     
               * Write down the size of this remote_repo_name.git folder. It is normal the size is already lower than the original .git folder since it is a mirror.
               * Now rewrite the history. In this example we remove all files larger than 10M (typically figures, .bag, .mat, .mp4 files) from history. For other examples see https://rtyley.github.io/bfg-repo-cleaner/.
               
                  .. code-block:: shell
                  
                     cd ~/Desktop
                     java -jar bfg.jar --strip-blobs-bigger-than 10M folder_name_mirrored_repo/remote_repo_name.git
                     
                  This process is very quick and a report is generated in the folder_name_mirrored_repo. You can see which files (and their size) have been removed.
               * If you are ok with the removed files, then do
               
                  .. code-block:: shell
                  
                        cd ~/Desktop/folder_name_mirrored_repo/remote_repo_name.git
                        git reflog expire --expire=now --all && git gc --prune=now --aggressive
                        
               * Write down the size of this remote_repo_name.git folder, it should be lower depedning on how much large files were removed.
               * Now push it back to github (don't put origin master this time, it won't work). 
               
                  .. code-block:: shell
                  
                        git push
                  
                  Since you rewrote history it is normal you won't see a commit on github. Indeed, you did not even create a commit.
               * Now clone your repo and check if the original .git folder is reduced in size and check if the code-block still builds and works as before. The cloning should also go faster now and less storage / bandwidth will be used. As an example this helped me to reduce the .git folder from 4.5G to 200M.
               
   * TODO check the use of [Distributing large binaries](https://docs.github.com/en/github/managing-large-files/working-with-large-files/distributing-large-binaries).
   * TODO checkout `this <https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/managing-git-lfs-objects-in-archives-of-your-repository>`__ 
   * TODO checkout https://github.com/git-lfs/git-lfs/blob/main/docs/man/git-lfs-migrate.1.ronn?utm_source=gitlfs_site&utm_medium=doc_man_migrate_link&utm_campaign=gitlfs
   * TODO find an elegant way to sotre large files (e.g. bag, mat CAD on cloud? while still being referencd via git)
   * TODO check https://docs.github.com/en/github/managing-large-files/versioning-large-files/moving-a-file-in-your-repository-to-git-large-file-storage
         

C++ Software Development
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Follow this \href{https://www.youtube.com/watch?v=vLnPwxZdW4Y}{quick C++ tutorial for beginners}. No need to do things, just follow it.


ROS Software Development
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Learn the basics and intermediate ROS concepts and tools by reading and testing the examples in the \emph{Mastering ROS for Robotics Programming} and its related github which can be found in our google drive. Read the following chapters in this book: ch1, ch2, ch3, ch4, NOT ch5, ch6, ch7, ch8 (nodelets very important), ch15. Although the books is written for ROS kinetic, just use ROS melodic on Ubuntu 18.
 
The CTU MRS Framework
^^^^^^^^^^^^^^^^^^^^^^^^^
TODO REFER TO SECTION ON THIS!

The software framework you will use during the project is based on \href{https://ctu-mrs.github.io/}{the MRS UAV system code from CTU Prague}. 
    * Read their wiki \href{https://ctu-mrs.github.io/} for the parts that are relevant for your thesis and install their code" mrs uav system" by following the steps found \href{https://github.com/ctu-mrs/mrs_uav_system#i-have-a-fresh-ubuntu-1804-and-want-it-quick-and-easy}{on their github repo}: "I have a fresh Ubuntu 18.04 and want it quick and easy". Than try to compile (i.e. build) the code by following these steps. You should NOT istall their linux setup. 
    * Learn the required skills from the links they provide.
    * In case you have problems only related to this software, please open a new issue \href{https://github.com/ctu-mrs/mrs_uav_system/issues}{here} or open a new discussion. Validate if the software builds without errors and without warnings. 
    * Run a some example scripts in the simulationfolder. Which ones do (not) work?
    * Read the paper of mrs uav system https://link.springer.com/article/10.1007/s10846-021-01383-5

Our droneswarm_brubotics Framework
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Read the relevant parts of our tutorial to learn to use the droneswarm_brubotics framework.
Please help us to improve the tutorual. If you struggled on some parts it means it was not writtin sufficiently well. 
Don't forget to commit your changes when updates this tutorial!
