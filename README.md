# AR_tag_detection_with_turtlebot3_gazebo
## To do Inference


- Launch turtlebot on gazebo. If you are doing it first time a window will show up and do nothing. wait for some time like 5 mins and restart gazebo. 


	```hitech@hitech:~/turtlebot_ws$ export TURTLEBOT3_MODEL=burger ```
	
	```hitech@hitech:~/turtlebot_ws$ source devel/setup.bash ```
	
	```hitech@hitech:~/turtlebot_ws$ roslaunch turtlebot3_gazebo turtlebot3_ar.launch```

- In another terminal launch teleop functionality,

	```hitech@hitech:~/turtlebot_ws$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch```

- In another terminal open rqt_image_view and use the teleop functionality to make sure AR Tag is in the field of view of robot. Clicking ```a``` can turn the robot arround and get the AT tag in robots FOV. 

	```hitech@hitech:~/turtlebot_ws$ rqt_image_view ```


- In another terminal, launch the ar_tag_alvar node to detect AR Tags
	
	```hitech@hitech:~/turtlebot_ws$ rosed ar_track_alvar pr2_indiv_no_kinect.launch ```

	- Change the marker size parameter to **50.0** in pr2_indiv_no_kinect.launch file.
	
	```hitech@hitech:~/turtlebot_ws$ roslaunch ar_track_alvar pr2_indiv_no_kinect.launch ```

- In another terminal open rviz and enable TFs and Marker to visualize

	```hitech@hitech:~/turtlebot_ws$ rviz ```
	
	- If the global status is not fixed, then change the Fixed frame parameter of Global options. 


# This explains how to setup the repo from scratch
## Installation of ros-kinetic-desktop-full version from this link <http://wiki.ros.org/kinetic/Installation/Ubuntu>. 

- Don't install gazebo from the website, it will wreck the whole setup. With the above desktop-full version it automatically install gazebo7. 

- #### Setting up gazebo environment. 

	- import the gazibo models to gazebo workspace by cloning the gazebo model database from <https://github.com/osrf/gazebo_models> and later copy all the models to ```/home/madhu16/.gazebo/models```. 

	- Copy the Marker model from given zip file to ```/home/madhu16/.gazebo/models```.

## Files to be replaced after creating the workspace (check this section after cloning all the necessary repos in next section)

1. **marker0** folder -> AR Tag converted to gazebo model.
2. **IndividualMarkersNoKinect.cpp** - Modified file to increase fps.
3. **turtlebot3_ar.world** - Custom gazebo world made specifically for better background. 
4. **turtlebot3_ar.launch** - Custom launch file for the custom gazebo world.
6. **turtlebot3_burger.gazebo.xacro** - Modified and given access to camera
7. **turtlebot3_burger.urdf.xacro**- Modified and given access to camera

## Creating turtlebot3 and AR Tag detection workspace 

- Create a dummy workspace :- 

	```hitech@hitech:~/$ mkdir -p turtlebot_ws/src```

	```hitech@hitech:~/$ cd turtlebot_ws```

	```hitech@hitech:~/turtlebot_ws$ catkin_make```

	```hitech@hitech:~/turtlebot_ws$ cd src```


- Clone **turtlebot3** repos and checkout the **kinetic branch**.

	```hitech@hitech:~/turtlebot_ws/src$ git clone -b kinetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs```
	
	```hitech@hitech:~/turtlebot_ws/src$ git clone -b kinetic-devel https://github.com/ms-iot/turtlebot3_simulations```
	
	```hitech@hitech:~/turtlebot_ws/src$ git clone -b kinetic-devel https://github.com/ms-iot/turtlebot3.git ```


- Clone **ar_track_alvar repo** for ar tag detection and checkout the **kinetic branch**.

	```hitech@hitech:~/turtlebot_ws/src$ cd ../../```
	
	```hitech@hitech:~/$ git clone -b kinetic-devel https://github.com/ros-perception/ar_track_alvar.git```
	
	```hitech@hitech:~/$ mv ar_track_alvar/ar_track_alvar ar_track_alvar/ar_track_alvar_msgs turtlebot_ws/src/```


- Build the downloaded packages using **catkin build** (or) **catkin_make**. 

	```hitech@hitech:~/turtlebot_ws$ catkin_make```

	- This might take some while, please monitor the system using **System Monitor** software in linux. If the system is stuck, wait for sometime and restart the system and build again with no other processes running.

- Verify the installation by launching **turtlebot3 node**. 

	```hitech@hitech:~/turtlebot_ws$ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch ```

- Check the above section for the list of files to be modified or added.

- Modifications in ar_track_alvar package for increasing fps of published marker topic :-

	Open **IndividualMarkersNoKinect.cpp** file from **ar_track_alvar/nodes/** folder and edit the **line no 88, 196** to change **1.0** to **0.03** . Go to **line no 277** and change max_frequency parameter to **30.0** . (or you can simply **replace** the whole file with the given modified file)


- Change the relevent things in turtlebot3 package.
	- Give access to camera and modify the resolution by replacing the **turtlebot3_burger.gazebo.xacro** and **turtlebot3_burger.urdf.xacro** files with the given modified files.

	- Add the given gazebo world file named **turtlebot3_ar.world** in **turtlebot3_simulations/turtlebot3_gazebo/worlds/**. This is a custom world file that I made specifically for good lighting condition.

	- Add the given launch file named **turtlebot3_ar.launch** in **turtlebot3_simulations/turtlebot3_gazebo/launch/**. This launch file uses the above world file. 


- Build the workspace finally,
	```hitech@hitech:~/turtlebot_ws$ catkin_make```


	```hitech@hitech:~/turtlebot_ws/src$ ```

## Do the Inference

- Launch turtlebot on gazebo. If you are doing it first time a window will show up and do nothing. wait for some time like 5 mins and restart gazebo. 


	```hitech@hitech:~/turtlebot_ws$ export TURTLEBOT3_MODEL=burger ```
	
	```hitech@hitech:~/turtlebot_ws$ source devel/setup.bash ```
	
	```hitech@hitech:~/turtlebot_ws$ roslaunch turtlebot3_gazebo turtlebot3_ar.launch```

- In another terminal launch teleop functionality,

	```hitech@hitech:~/turtlebot_ws$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch```

- In another terminal open rqt_image_view and use the teleop functionality to make sure AR Tag is in the field of view of robot. Clicking ```a``` can turn the robot arround and get the AT tag in robots FOV. 

	```hitech@hitech:~/turtlebot_ws$ rqt_image_view ```


- In another terminal, launch the ar_tag_alvar node to detect AR Tags
	
	```hitech@hitech:~/turtlebot_ws$ rosed ar_track_alvar pr2_indiv_no_kinect.launch ```

	- Change the marker size parameter to **50.0** in pr2_indiv_no_kinect.launch file.
	
	```hitech@hitech:~/turtlebot_ws$ roslaunch ar_track_alvar pr2_indiv_no_kinect.launch ```

- In another terminal open rviz and enable TFs and Marker to visualize

	```hitech@hitech:~/turtlebot_ws$ rviz ```
	
	- If the global status is not fixed, then change the Fixed frame parameter of Global options. 

## Generating custom AR Tag gazebo models (optional).

- Generate custom AR Tag images from this instructions from here or <http://wiki.ros.org/ar_track_alvar>
	
	```hitech@hitech:~/turtlebot_ws$ rosrun ar_track_alvar createMarker ```
	
- Generate AR Tag gazebo models from AR Tag images by following this link <https://github.com/mikaelarguedas/gazebo_models>
	
	- Follow the readme file of this repo to generate gazebo models of AR tag images.  


