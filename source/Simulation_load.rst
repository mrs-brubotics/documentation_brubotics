Step to reproduce spawning of a load, still need to adapt syntax
================================================================
\section{Creation of a new world with payload and automate it to start with simulation}
---------------------------------------------------------------------------------------
\bc{very unclear section title. Be more specific. Also think how this chapter is different from chapter on custom simulation world. That chapter explain more generically how to change the world. This chapter should be more specific.} \pp{Changed, chapter is meant to create our environment with payload and make it start with the ./start.sh}
To create a new simulation environment, follow these steps:
\begin{enumerate}
    \item Make a new folder in the directory:\\ "/workspace/src/droneswarm\_brubotics/ros\_packages/testing\_brubotics/tmux\_scripts"
    \item Copy the files ("start.sh", "layout.json" and "session.yml") of an other example (from brubotics or mrs) in your new folder. Take a simulation as close as possible to what you want to achieve (e.g.:\bc{use e.g.}\pp{Done} one drone gps). 
    \item Launch your simulation. \bc{I think up to this part should be explained earlier in the tutorial. At this point everyone should know this and not be reminded that a simulation can be started like that.} \pp{Done}
    \item If you want to create a new world \bc{see previous chapter on creating new worlds, this "general" explanation belongs there}, copy an existing world file in in the folder you created in ".../tmux\_scripts" . The location of the standard mrs world files are in following directory:\\
    "/git/simulation/ros\_packages/mrs\_gazebo\_common\_resources". For our situation, we started from the standard grass plane. \bc{be more specific about which files you base yourself on for the specific task you have.}\pp{Done}
    \item Adapt your file by adding models, e.g. boxes or trees. We added a box that will be used as payload. Find exemples of codes to implement this models in other .world files or the internet. \bc{no no need list this code if you do not talk about specific parts in the code. Better to insert a link to this file on our github. Try to explain which parameters are important to set for your purpose. Now it is again "quite generic". It does not directly explain the specifics, although they might be in the code.}\pp{Done, not pushed yet so we still have to add link}
    \item Change "world\_name:=grass\_plane" in the following lines in the "Session.yml" file: \bc{"something like" is very unclear. Be more specific. In this line to only specifics is grass plane. Indeed, you gonna tell this story with the easiest world there is: a grass plane. No need to add more complex worlds in this stage.}\pp{Done}
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
      - gazebo:
      layout: tiled
      panes:
        - waitForRos; roslaunch mrs_simulation simulation.launch world_name:=grass_plane gui:=true
    \end{lstlisting}
    to (adapt text in LARGE LETTERING)
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
      - gazebo:
      layout: tiled
      panes:
        - waitForRos; roslaunch mrs_simulation simulation.launch world_file:='/home/NAME OF COMPUTER SESSION/workspace/src/droneswarm_brubotics/ros_packages/testing_brubotics/tmux_scripts/NAME OF FOLDER/NAME OF WORLD FILE.world' gui:=true
    \end{lstlisting}
    \bc{the above is a more generic way to tell that you can dd multiple world files. I agree with that. It is relevant for the previous chapter on world modelling. For this part i don't see what you tell more then the fact you are using a grass plane.}\pp{This is done to show how to add a payload to your world and make sure it starts with your simulation. We will also need the world file to add the plugin}
\end{enumerate}

\section{Link\_Attacher}
------------------------
\subsection{Installation}
To be able to attach a load to your drone, follow next steps:
\begin{enumerate}
    \item Exectute the following commands to clone the extra recources file of MRS:
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
     cd ~/workspace/src/
     git clone https://github.com/ctu-mrs/mrs_gazebo_extras_resources
    \end{lstlisting}
    \bc{TODO: the fact that this is not available by default when installing the mrs uav system, we need to think of a way to directly add it when installing out droneswarm project. Some kind of required dependcy. Easiest way is to write in the instructions. to install this package before installing the droneswarm brubotics code.}
    \item Add the plugin of the link\_attacher in the .world file:
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
     <plugin name="mrs_gazebo_link_attacher_plugin" filename="libMRSGazeboLinkAttacherPlugin.so"/> 
     \end{lstlisting}
\end{enumerate}

\subsection{Creation of a link}
\bc{make title more clear: Use of what?}\pp{Done}
Now you can use the link attacher plugin in your simulation. To be able to use the plugin, there must be an object in your .world file to attach to your drone (a box for example):
\begin{enumerate}
    \item Start your simulation from previous chapter. \bc{trivial. better to explain which simulation you are running here. give the path.}\pp{Done}
    \item When the simulation has started, move your drone above the object you want to connect it with. The distance between the drone and the object will be the length of the link. \bc{I assume you want to fly it above the object? This important information is missing. Otherwise a newbie might try to fly the drone in the obstacle.}\pp{Done}
    \item Create the link by performing following commands in a new shell tab, while adapting all the names between parentheses' \bc{these are no brackets. these are []}\pp{Done} to your situation. The correct model and link names can be seen in gazebo.
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
    rosservice call /link_attacher_node/attach "model_name_1: 'uav1' link_name_1: 'base_link' model_name_2: 'unit_box' link_name_2: 'link1'"
     \end{lstlisting}
     This link will create a distance constraint, between the links of the two models. This means the objects will always stay at a same distance from each other. The link will however not be visible. The links are placed in the center of mass of a standard object. We will later, in section \ref{XacroPayload}, see how links can be placed at other places than the center of mass.
     \bc{explain what this link physically does. explain where the link names are located in the drone and the object. Is it in the centre off mass or somewhere else?}\pp{Done}
    \item If the connection succeeded, the message "ok: True" will be given. It could not succeed if you wrote the names of your links and models wrong. \bc{is there any moment you saw it did not work? If so explain in which situation?}\pp{Done}
    \item You can also change the joint type by adding "joint\_type: ' '" as shown below. The possible choices are "revolute", "ball", "gearbox", "prismatic", "revolute2", "universal", "piston", "fixed". If you do not specify the joint type, it will be a revolute joint. The joint type you define will be the joint connecting the first model with the link, the connection of the second model to the link, will be fixed.  
    \begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
    rosservice call /link_attacher_node/attach_type "model_name_1: 'uav1' link_name_1: 'base_link' model_name_2: 'unit_box' link_name_2: 'link1' joint_type: 'ball'"
     \end{lstlisting}
    In our situation we want a ball joint (spherical joint), to approach a cable on a hinge.\bc{you give the explanation of adding joint types possible, but you don't explain which type we want in your situation. }\pp{Done}\\
    \item Now you can move your drone up to see your payload take off. Try moving your drone sideways, you will see the payload is not implemented yet in the control and will oscillate. 
\end{enumerate}

\begin{figure}[H]
    \centering
    \includegraphics[width=0.5\linewidth]{Chapter_Simulation_load_transportation/Pictures/Link_attacher.png}
    \caption{Drone with attached payload}
    \label{fig:Drone_attached_payload}
\end{figure}

\bc{add a picture of what the user should see? or do with the drone, should he/she give a command to see some swing dynamics?}\pp{Done}
\newpage
\section{Model your payload with an URDF file}
Instead of spawning the box in the world file as done previously, it is possible to make an urdf file of the payload. This has the advantage that you can define more comlex connections of multiple objects and add joints between elements.

\subsection{Create urdf file}
Open a blank file and save it as MODELNAME.urdf \bc{why bold?} \rn{changed it}, for the MODELNAME you can choose what you want. Place the urdf file in an existing package or make a new package. For instant just place everything in the new folder you created in the testing\_brubotics package. In the following code we have an example to model a box \bc{add link so user can follow the structure of your packages}. You can copy and paste this code in the blank urdf file, then save the document. TIP: paste the code from the source of overleaf to keep the same structure, otherwise can cause problems! \bc{just refer to the file in the code. No need to paste code here, unless those specific section you want to give explantions about. }

\begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
<?xml version="1.0" ?>
<robot name="ROBOTNAME" xmlns:xacro="http://www.ros.org/wiki/xacro">
    
    <!-- 1st link -->
  <link name="link_chassis">
    <pose>0 0 0.1 0 0 0</pose>
    
    <inertial>
      <mass value="5"/>
      <origin xyz="0 0 0.1" rpy="0 0 0"/>
      <inertia ixx="0.0395" ixy="0" ixz="0" iyy="0.106" iyz="0" izz="0.1062"/>
    </inertial>
    
    <collision name="collision_chassis">
      <geometry>
        <box size="1 1 2"/>
      </geometry>
    </collision>
    
    <visual>
      <origin rpy="0 0 0" xyz="0 0 0"/>
      <geometry>
        <box size="1 1 2"/>
      </geometry>
    </visual>
  </link>
 
</robot>
\end{lstlisting}
The <xml> line is a standard line then in the second line of code you have to give a name to your robot (ROBOTNAME), you can change what you want for example "payload". Start the robot description with <robot>. The next step is to make the links and joints. There are some sub modules like inertial, collision and visual. Again you can name them how you want. The sub modules can be modified and the collision and visual do not have to be the same. More info can be found on \hyperlink{blue}{http://wiki.ros.org/urdf/XML/link}. Finally, close the robot description with </robot>. \bc{why do you chose a mass of 5, why that inertia?} \rn{was just a first example to show that you can choose what you want}

\subsection{Create a launch file}
Now that you have created the urdf file, it needs to be executed. Therefore we use a launch file. Again open a blank file and save it as NAME.launch, with "NAME" that can be what you want. \bc{the goal of the reader is not to copy paste code from this report to reproduce steps. The idea is you give them the commands to run and refer to all files (e.g. launch files, session files, ...) so the user can run it.} Place it in the folder with all the other documents you created in testing\_brubotics \bc{just giving the link makes all this more tangible}. Below an example of a launch file is shown, you can copy paste this code in your launch file. TIP: paste from source of overleaf not from pdf is easier!

\begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
<?xml version="1.0"?>
<launch>
    <param name="robot_description" command="$(find xacro)/xacro '$(find testing_brubotics)/tmux_scripts/FOLDERNAME/MODELNAME.urdf'" />
    
    <arg name="x" default="0"/>
    <arg name="y" default="0"/>
    <arg name="z" default="1.5"/>
    
    <node name="NODENAME" pkg="gazebo_ros" type="spawn_model" output="screen"
          args="-urdf -param robot_description -model ROBOTNAME -x $(arg x) -y $(arg y) -z $(arg z)" />
          
</launch>
\end{lstlisting}

Again, the first line of code is as standard line that has to be put. Start the launch file with <launch> on the second line. The param name="robot\_description" is a package in ROS and cannot be changed. Then the command find xacro is executed, this tries to find the urdf file in the path you provide. Change the correct names that are in UPPERCASE to your directory and urdf file name!\\

Then some arg are defined, "x, y and z", this is were the urdf file will be spawned. You can change those values. Finally, you create a node with "NODENAME" that can be changed to what you want for example, spawn\_payload. The pkg used is gazebo\_ros with a certain type and the result is shown on the screen. The arguments are given to the urdf file where you need to change the ROBOTNAME, to the name you gave in the urdf file!\\

To test if everything works as expected launch a simulation (./start.sh in the right folder). Then execute the launch file by opening a new terminal and pasting the following command (change the name to your NAME.launch file). You should see a box spawn like on the figure.

\begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
    roslaunch testing_brubotics NAME.launch
\end{lstlisting}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=.8\linewidth]{Chapter_Simulation_load_transportation/Pictures/urdf_install.png}
    \caption{launching the urdf file in a new terminal (change the load.launch to the name of your launch file) \bc{so which xyz did you enter to get this picture. This is valuable for the reader to check if he cna reprduce.}\rn{Spawned the block in the origin xyz= 0 0 0}}
    \label{fig:Launching_urdf}
\end{figure}

\subsection{Automate this using tmux}
\bc{there is a lot of explanation and copy paste of code here, but too much for what you want to tell. Just refer to the session file and say which lines you added, give the explanation required to understand why you added those lines.}\rn{Done} Instead of opening a new terminal it is possible to do it with the rest of the simulation. Open for that your session.yml file in your directory. Add the lines that are indicated below between the spawn and control code, and change the NAME.launch to your actual launch file. Save then exit the document. Now when executing ./start.sh you should see the box spawn in your world. The lines added will execute the launch file.

\begin{lstlisting}[basicstyle=\ttfamily,breaklines=true, columns=fullflexible, backgroundcolor=\color{light-gray}]
  - load:
      layout: tiled
      panes:
        - waitForSimulation; roslaunch testing_brubotics NAME.launch
\end{lstlisting}

