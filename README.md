# ROS2-PX4-Keyboard-Control-with-Live-Camera-Streaming
A complete ROS2 Jazzy and PX4 SITL project demonstrating keyboard control of an X500 drone in Gazebo Harmonic with real-time camera streaming using ROS2 image transport. The project includes Micro XRCE-DDS communication, PX4 Offboard Control, Gazebo simulation, ROS2 image bridge, and live camera visualization using rqt_image_view.
ROS2 Autonomous Drone Simulation using PX4 + Gazebo Harmonic
Project Overview

This project demonstrates the complete simulation of a quadcopter using PX4 Autopilot, ROS2 Jazzy, and Gazebo Harmonic on Ubuntu 24.04.

The drone can be controlled manually using the keyboard while streaming live camera images to ROS2. Both RGB camera and LiDAR sensors are integrated, making the project suitable as a foundation for autonomous navigation, SLAM, obstacle avoidance, and future AI applications.

Features
PX4 SITL Flight Controller
Gazebo Harmonic Simulation
ROS2 Jazzy Integration
Micro XRCE DDS Communication
Keyboard Teleoperation
Live RGB Camera Streaming
Depth Camera Support
2D LiDAR Support
ROS-Gazebo Topic Bridging
Ready for SLAM
Ready for Autonomous Navigation
Software Used
Ubuntu 24.04
ROS2 Jazzy
Gazebo Harmonic
PX4 Autopilot
Micro XRCE DDS Agent
ros_gz_bridge
OpenCV
rqt_image_view
Project Architecture
Keyboard
      │
      ▼
ROS2 Keyboard Node
      │
      ▼
PX4 Offboard Commands
      │
      ▼
Micro XRCE DDS
      │
      ▼
PX4 SITL
      │
      ▼
Gazebo Harmonic
      │
      ├──────── RGB Camera
      │              │
      │              ▼
      │        ros_gz_bridge
      │              │
      │              ▼
      │        ROS2 Image Topic
      │              │
      │              ▼
      │      rqt_image_view
      │
      └──────── LiDAR
                     │
                     ▼
             ros_gz_bridge
                     │
                     ▼
             ROS2 LaserScan
Workspace Structure
px4_ros_ws/

src/

├── drone_keyboard/

├── drone_controller/

├── drone_control/

├── px4_msgs/

└── px4_ros_com/

PX4-Autopilot/

Gazebo Models/

Launch Files/
Starting the Simulation
Terminal 1

Start DDS Agent

source /opt/ros/jazzy/setup.bash

MicroXRCEAgent udp4 -p 8888

Expected

running...

session established
Terminal 2

Start PX4

cd ~/drone_sim/PX4-Autopilot

make px4_sitl gz_x500_depth

or

make px4_sitl gz_x500_lidar_2d

Wait until

pxh>

appears.

PX4 Parameters

Inside PX4 console

param set COM_RCL_EXCEPT 4

param set COM_ARM_WO_GPS 1

param set NAV_DLL_ACT 0

param save

Verify

commander check
Terminal 3

Bridge RGB Camera

source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
Terminal 4

Bridge Depth Camera

source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/depth_camera@sensor_msgs/msg/Image@gz.msgs.Image
Terminal 5

LiDAR Bridge

source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_lidar_2d_0/link/link/sensor/lidar_2d_v2/scan@sensor_msgs/msg/LaserScan@gz.msgs.LaserScan
Terminal 6

Keyboard Control

source /opt/ros/jazzy/setup.bash

source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control
Terminal 7

Open Live Camera

source /opt/ros/jazzy/setup.bash

rqt_image_view

Select

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

You should now see the live RGB video from the drone.

Verify Camera
ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

Expected

6–20 Hz
Verify Depth Camera
ros2 topic hz /depth_camera

Expected

6–20 Hz
Verify LiDAR
ros2 topic hz \
/world/default/model/x500_lidar_2d_0/link/link/sensor/lidar_2d_v2/scan

Expected

30 Hz
Keyboard Controls
Key	Function
T	Arm
Y	Takeoff
W	Forward
S	Backward
A	Left
D	Right
I	Up
K	Down
J	Rotate Left
L	Rotate Right
Space	Hover
X	Land
ROS2 Topics

RGB Camera

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

Camera Info

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info

Depth Image

/depth_camera

Point Cloud

/depth_camera/points

LiDAR Scan

/world/default/model/x500_lidar_2d_0/link/link/sensor/lidar_2d_v2/scan

PX4 Topics

/fmu/in/*
/fmu/out/*
System Verification Commands

DDS

MicroXRCEAgent udp4 -p 8888

PX4

commander check

Camera Rate

ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

Depth

ros2 topic hz /depth_camera

LiDAR

ros2 topic hz \
/world/default/model/x500_lidar_2d_0/link/link/sensor/lidar_2d_v2/scan

Camera Viewer

rqt_image_view
Future Improvements
Autonomous Navigation
SLAM Toolbox
RTAB-Map
Obstacle Avoidance
Path Planning
Mission Planning
Object Detection using YOLO
AI-based Navigation
Multi-Drone Simulation
Technologies
Ubuntu 24.04
ROS2 Jazzy
PX4 Autopilot
Gazebo Harmonic
Micro XRCE DDS
ros_gz_bridge
OpenCV
C++
Python
Author

Vikas Kolhe

Project Title:
ROS2 Autonomous Drone Simulation with PX4, Gazebo Harmonic, Keyboard Teleoperation, Live Camera Streaming, and LiDAR Integration
