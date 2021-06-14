
# BookBot

This ROS package implements autonomous navigation using ROS Navigation Stack for custom differential drive robot named "BookBot" with the aim of collecting books and leave them at the reception of the bookstore.
SLAM algorithm is used to map an unknown environment, in particular a bookstore. Keyboard is used to teleoperate the robot in Gazebo in order to generate a map for the global planner of the navigation stack. The robot is equipped with a planar laser scan (Hokuyo Laser) which allows to sense the environment.
The environment present both static and dynamic obstacles.

[Simulation Video](https://youtu.be/0HSZ3LU_FVI)

## Mapping the indoor environment

The package uses [slam_gmapping](http://wiki.ros.org/slam_gmapping) to build a map of the environment.

1. Load the gazebo world and spawn the robot. World model is the bookstore.
	```
	$ roslaunch bookbot gazebo.launch 
	```
2. Launch the **slam_gmapping** node.
	```
	$ roslaunch bookbot gmapping.launch
	```
3. Move the robot around using keyboard teleoperation
	 ```
	 $ roslaunch bookbot key_teleop.launch
	 ```
4. Move the robot in your environment until a good map is created. 
5. Save the map using:
	```
	$ rosrun map_server map_saver -f ~/<folder>/<map_name>
	``` 
	
## Autonomous Navigation
This package uses the [ROS Navigation stack](http://wiki.ros.org/navigation) to autonomously navigate through the map created using gmapping. 
  
0. To use generated map, edit ```/launch/amcl_move_base.launch``` and add map .yaml location and name to map_server node launch.
1. Load the robot in gazebo environment:
	```
	$ roslaunch bookbot gazebo.launch 
	```
2. Start the **amcl**, **move_base** and **rviz** nodes:
	```
	$ roslaunch bookbot amcl_move_base.launch
	```
3. Run the **nav_goal.py** node to move the robot along defined waypoints and collect books:
	```
	$ rosrun bookbot nav_goal.py
	``` 

##  ROS package to install

Before using this package, make sure you have installed the following ROS packages:

1. Install the ROS [navigation](http://wiki.ros.org/navigation) package:
	``` $ sudo apt-get install ros-noetic-navigation``` 
2. Install the ROS [slam_gmapping](http://wiki.ros.org/gmapping) package:
	``` $ sudo apt-get install ros-noetic-slam-gmapping``` 
3. Install the ROS [teb_local_planner](http://wiki.ros.org/teb_local_planner) package:
	``` $ sudo apt-get install ros-noetic-teb-local-planner``` 

##  Notice

Make sure to change the absolute directory properly in **nav_goal.py** where books are spawned.
