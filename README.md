# **CarND-Capstone-Project-9** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

Overview
---

This repository is the capstone project of Udacity's Self-Driving Car NanoDegree program. The project integrates multiple components of a self-driving car into a system that's then used to drive **Carla** autonomously in a simulated environment. System integration is accomplished by means of **ROS** - whereby components are implemented as **ROS nodes**.

Figure 1 is a system architecture diagram showing the ROS nodes and topics used in the project. The ROS nodes and topics shown in the diagram are described briefly in the sections below, More detail is provided for each node in classroom concepts of this final module.


Directory Structure
---

```
root
|   requirements.txt
|   Dockerfile
|  
|___ros
    |___launch
        | site.launch
        | styx.launch
    |___src   
        |___camera_info_publisher
        |___styx
        |___tl_detector
        |___twist_controller
        |___waypoint_follower
        |___waypoint_loader
        | CMakeLists.txt
        | .catkin_workspace
    |___data
    |___imgs
```


System Integration
---

![my_image1](https://user-images.githubusercontent.com/76077647/134168734-3ec501c6-2c51-46da-a159-a6c0e4f15d9d.JPG)

**Fig 1**: Integrated system


ROS Node: Traffic Light Detection Node
---

This node takes in data from the ```/image_color```, ```/current_pose```, and ```/base_waypoints``` topics and publishes the locations to stop for red traffic lights to the ```/traffic_waypoint``` topic.

The ```/current_pose``` topic provides the vehicle's current position, and ```/base_waypoints``` provides a complete list of waypoints the car will be following.

**NOTE**: Please download the pretrained traffic light model [here](https://drive.google.com/drive/folders/1T7psb-20oYDoodONcD2_9buqpVlw5dW8?usp=sharing) & save it inside the **model** subdirectory. 

![my_image2](https://user-images.githubusercontent.com/76077647/134168273-314ae4f9-d636-42a9-bac8-85a88aa18947.JPG)

**Fig 2**: Traffic light detection node


ROS Node: Waypoint Updater Node
---

The purpose of this node is to update the target velocity property of each waypoint based on traffic light and obstacle detection data. This node will subscribe to the ```/base_waypoints```, ```/current_pose```, ```/obstacle_waypoint```, and ```/traffic_waypoint``` topics, and publish a list of waypoints ahead of the car with target velocities to the ```/final_waypoints``` topic.

![my_image3](https://user-images.githubusercontent.com/76077647/134168463-9c6a774b-a1ef-40b5-94ab-b7330b126d6d.JPG)

**Fig 2**: Waypoint updater node


ROS Node: DBW Node
---

Carla is equipped with a drive-by-wire (dbw) system, meaning the throttle, brake, and steering have electronic control. The dbw_node subscribes to the ```/current_velocity``` topic along with the ```/twist_cmd``` topic to receive target linear and angular velocities. Additionally, this node will subscribe to ```/vehicle/dbw_enabled```, which indicates if the car is under dbw or driver control. This node will publish throttle, brake, and steering commands to the ```/vehicle/throttle_cmd```, ```/vehicle/brake_cmd```, and ```/vehicle/steering_cmd``` topics.

![my_image4](https://user-images.githubusercontent.com/76077647/134168692-e9d43dc7-887d-4b78-bfe9-20cdc8b132ea.JPG)

**Fig 3**: DBW node


Installation
---

Please use one of the two installation options, either native or docker installation.
Native Installation

- Be sure that your workstation is running Ubuntu 16.04 Xenial Xerus or Ubuntu 14.04 Trusty Tahir. [Ubuntu downloads can be found here](https://www.ubuntu.com/download/desktop).

- If using a Virtual Machine to install Ubuntu, use the following configuration as minimum:
  - 2 CPU
  - 2 GB system memory
  - 25 GB of free hard drive space

  The Udacity provided virtual machine has ROS and Dataspeed DBW already installed, so you can skip the next two steps if you are using this.

- Follow these instructions to install ROS
  - [ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu) if you have Ubuntu 16.04.
  - [ROS Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu) if you have Ubuntu 14.04.

- [Dataspeed DBW](https://bitbucket.org/DataspeedInc/dbw_mkz_ros)
  - Use this option to install the SDK on a workstation that already has ROS installed: [One Line SDK Install (binary)](https://bitbucket.org/DataspeedInc/dbw_mkz_ros/src/81e63fcc335d7b64139d7482017d6a97b405e250/ROS_SETUP.md?fileviewer=file-view-default)

- [Download the Udacity Simulator](https://github.com/udacity/CarND-Capstone/releases).

Docker Installation
---

[Install Docker](https://docs.docker.com/engine/installation/)

Build the docker container

```
docker build . -t capstone
```

Run the docker file

```
docker run -p 4567:4567 -v $PWD:/capstone -v /tmp/log:/root/.ros/ --rm -it capstone
```

Port Forwarding
---

To set up port forwarding, please refer to the [instructions from term 2](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/16cf4a78-4fc7-49e1-8621-3450ca938b77)

Usage
---

1. Clone the project repository
```
git clone https://github.com/udacity/CarND-Capstone.git
```

2. Install python dependencies

```
cd CarND-Capstone
pip install -r requirements.txt
```
3. Make and run styx

```
cd ros
catkin_make
source devel/setup.sh
roslaunch launch/styx.launch
```
4. Run the simulator

Real world testing
---

1. Download [training bag](https://drive.google.com/file/d/0B2_h37bMVw3iYkdJTlRSUlJIamM/view?usp=sharing) that was recorded on the Udacity self-driving car (a bag demonstraing the correct predictions in autonomous mode can be found here)
2. Unzip the file

```
unzip traffic_light_bag_files.zip
```

3. Play the bag file

```
rosbag play -l traffic_light_bag_files/loop_with_traffic_light.bag
```

4. Launch your project in site mode

```
cd CarND-Capstone/ros
roslaunch launch/site.launch
```

5. Confirm that traffic light detection works on real life images

References & Credits
---

* udacity (self driving car nanodegree - CarND-Capstone)
* course peers
