# ROS2-PX4-Keyboard-Control-with-Live-Camera-Streaming
A complete ROS2 Jazzy and PX4 SITL project demonstrating keyboard control of an X500 drone in Gazebo Harmonic with real-time camera streaming using ROS2 image transport. The project includes Micro XRCE-DDS communication, PX4 Offboard Control, Gazebo simulation, ROS2 image bridge, and live camera visualization using rqt_image_view.
Overview

This project demonstrates how to control a PX4 drone in Gazebo Harmonic using ROS2 Jazzy.

The drone can

Takeoff
Land
Move Forward
Move Backward
Move Left
Move Right
Ascend
Descend
Rotate
Stream live camera images

Everything runs completely inside simulation.

Software Used
Software	Version
Ubuntu	24.04
ROS2	Jazzy
Gazebo	Harmonic
PX4	Latest Main
Micro XRCE DDS Agent	Latest
Python	3.12
Project Structure
px4_ros_ws/

src/

drone_keyboard/
    keyboard_control.py

drone_controller/

drone_control/

px4_ros_com/

px4_msgs/
Workspace
~/px4_ros_ws
Install Dependencies
sudo apt update

Install ROS

sudo apt install ros-jazzy-desktop

Install Gazebo

sudo apt install gz-harmonic

Install PX4 dependencies

bash ./Tools/setup/ubuntu.sh

Install DDS Agent

sudo apt install ros-jazzy-micro-ros-agent

Install Image Viewer

sudo apt install ros-jazzy-rqt-image-view

Install ros_gz_bridge

sudo apt install ros-jazzy-ros-gz
Build Workspace
cd ~/px4_ros_ws

colcon build

source install/setup.bash
Startup Procedure
Terminal 1

DDS Agent

source /opt/ros/jazzy/setup.bash

MicroXRCEAgent udp4 -p 8888

Expected

Running...
Session Established
Terminal 2

Start PX4

cd ~/drone_sim/PX4-Autopilot

make px4_sitl gz_x500_depth

Wait until

pxh>

appears.

Terminal 3

Bridge Camera

source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
Verify Topics
ros2 topic list

Expected

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info
Check Camera Frequency
ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

Expected

6–10 Hz
Terminal 4

Run Keyboard Controller

source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control
Terminal 5

View Live Camera

source /opt/ros/jazzy/setup.bash

rqt_image_view

Choose

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

You should now see live video from the drone.

Keyboard Controls
Key	Action
T	Takeoff
L	Land
W	Forward
S	Backward
A	Left
D	Right
Q	Rotate Left
E	Rotate Right
R	Up
F	Down
X	Stop
Verify ROS Communication

List topics

ros2 topic list

Check nodes

ros2 node list

Check PX4 connection

ros2 topic list | grep fmu
Camera Verification
ros2 topic echo \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image --once
Useful Commands

DDS Agent

MicroXRCEAgent udp4 -p 8888

Check Image Rate

ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

View Camera

rqt_image_view

Check PX4 Topics

ros2 topic list | grep fmu
Features
PX4 SITL
Gazebo Harmonic Simulation
ROS2 Jazzy
Keyboard Teleoperation
Live RGB Camera
DDS Communication
ROS2 Image Transport
Real-Time Video Streaming
Technologies
ROS2 Jazzy
PX4
Gazebo Harmonic
Python
Micro XRCE DDS
ros_gz_bridge
rqt_image_view
Future Improvements
SLAM
LiDAR Mapping
Autonomous Navigation
Obstacle Avoidance
Mission Planning
Waypoint Navigation
Recommended GitHub folder structure
ROS2-PX4-Keyboard-Control-with-Live-Camera-Streaming/
│
├── README.md
├── LICENSE
├── images/
│   ├── drone.png
│   ├── gazebo.png
│   ├── live_camera.png
│   ├── keyboard_control.png
│   └── architecture.png
├── src/
│   └── drone_keyboard/
├── launch/
├── scripts/
├── docs/
│   ├── Installation.md
│   ├── Startup_Guide.md
│   └── Troubleshooting.md
└── videos/
