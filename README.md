# ROS2-PX4-Keyboard-Control-with-Live-Camera-Streaming
A complete ROS2 Jazzy and PX4 SITL project demonstrating keyboard control of an X500 drone in Gazebo Harmonic with real-time camera streaming using ROS2 image transport. The project includes Micro XRCE-DDS communication, PX4 Offboard Control, Gazebo simulation, ROS2 image bridge, and live camera visualization using rqt_image_view.
ROS2 Autonomous Drone Simulation using PX4 + Gazebo Harmonic

ROS2-PX4-Drone-Keyboard-Control-Live-Camera/

│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── Installation.md
│   ├── Startup_Guide.md
│   ├── Camera_Streaming.md
│   ├── Keyboard_Control.md
│   ├── Troubleshooting.md
│
├── images/
│   ├── architecture.png
│   ├── drone.png
│   ├── gazebo.png
│   └── camera_stream.png
│
├── scripts/
│   ├── start_px4.sh
│   ├── start_bridge.sh
│   ├── start_camera.sh
│   ├── start_keyboard.sh
│   └── startup_all.sh
│
├── commands/
│   ├── startup_commands.md
│   ├── camera_commands.md
│   ├── keyboard_commands.md
│   └── troubleshooting.md
│
└── src/

# ROS2 PX4 Drone Keyboard Control with Live Camera Streaming

This project demonstrates how to control a PX4 drone inside Gazebo Harmonic using ROS2 Jazzy while simultaneously streaming the onboard RGB camera.

The project uses:

- Ubuntu 24.04
- ROS2 Jazzy
- Gazebo Harmonic
- PX4 Autopilot
- Micro XRCE DDS Agent
- ROS-Gazebo Bridge

Features

✔ Keyboard Control

✔ Live Camera Streaming

✔ ROS2 Image Topic

✔ Gazebo Harmonic Simulation

✔ PX4 Offboard Communication

---

## System Architecture

Gazebo
        │
        ▼
PX4 SITL
        │
        ▼
DDS Agent
        │
        ▼
ROS2
        │
 ┌─────────────┐
 │Keyboard Node│
 └─────────────┘
        │
        ▼
Drone Movement

Camera

Gazebo Camera

↓

ROS-GZ Bridge

↓

ROS2 Image Topic

↓

Image Viewer

Workspace
~/drone_sim/
        PX4-Autopilot/

~/px4_ros_ws/
        src/

Software Versions
Ubuntu 24.04

ROS2 Jazzy

Gazebo Harmonic

PX4 Stable

Micro XRCE DDS Agent

Launch Order
1 DDS Agent

↓

2 PX4 + Gazebo

↓

3 Camera Bridge

↓

4 Keyboard

↓

5 Image Viewer

Result
Keyboard

↓

Drone flies

↓

Camera streams live

↓

Image Viewer displays video

Folder docs/Installation.md
# Installation Guide

Install ROS2 Jazzy

```bash
sudo apt update
sudo apt install ros-jazzy-desktop -y
```

---

Install Gazebo Harmonic

```bash
sudo apt install gz-harmonic
```

---

Clone PX4

```bash
mkdir -p ~/drone_sim

cd ~/drone_sim

git clone https://github.com/PX4/PX4-Autopilot.git

cd PX4-Autopilot

bash ./Tools/setup/ubuntu.sh
```

---

Compile

```bash
make px4_sitl
```
docs/Keyboard_Control.md

# Keyboard Control

Launch

```bash
source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control
```

Controls

```
W

Forward

S

Backward

A

Left

D

Right

Q

Rotate Left

E

Rotate Right

R

Up

F

Down

T

Takeoff

L

Land
```

docs/Camera_Streaming.md

# Live Camera

Bridge

```bash
source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
```

Check

```bash
ros2 topic list
```

Expected

```
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

View

```bash
rqt_image_view
```

Choose

```
IMX214/image
```
scripts/start_px4.sh
#!/bin/bash

source /opt/ros/jazzy/setup.bash

cd ~/drone_sim/PX4-Autopilot

make px4_sitl gz_x500_depth
scripts/start_bridge.sh
#!/bin/bash

source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
scripts/start_keyboard.sh
#!/bin/bash

source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control
commands/startup_commands.md
# Startup

Terminal 1

```bash
source /opt/ros/jazzy/setup.bash

MicroXRCEAgent udp4 -p 8888
```

---

Terminal 2

```bash
cd ~/drone_sim/PX4-Autopilot

make px4_sitl gz_x500_depth
```

---

Inside PX4

```text
param set COM_RCL_EXCEPT 4

param set COM_ARM_WO_GPS 1

param set NAV_DLL_ACT 0

param save
```

---

Terminal 3

```bash
source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
```

---

Terminal 4

```bash
source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control
```

---

Terminal 5

```bash
rqt_image_view
```

Choose

```
IMX214/image
```

PART 3 — Keyboard Controller (ROS 2)

# Keyboard Controller for PX4 Drone

## Overview

This package provides a simple keyboard interface for manually controlling the PX4 drone inside Gazebo Harmonic.

Instead of using QGroundControl or a joystick, keyboard commands are converted into ROS 2 messages and sent to PX4 through Offboard mode.

This controller was mainly used for:

- Manual flight testing
- Mapping environments
- Camera testing
- LiDAR testing
- Autonomous navigation preparation

---

# Features

✔ Arm Drone

✔ Takeoff

✔ Land

✔ Forward Motion

✔ Backward Motion

✔ Left Motion

✔ Right Motion

✔ Up

✔ Down

✔ Yaw Left

✔ Yaw Right

✔ Emergency Stop

---

# Package Structure

drone_keyboard/

├── package.xml

├── setup.py

├── drone_keyboard/

│ ├── init.py

│ └── keyboard_control.py

└── resource/

---

# Dependencies

Install keyboard library

sudo apt install python3-pynput

or

pip install pynput

ROS2 Packages

source /opt/ros/jazzy/setup.bash

---

# Build Workspace

cd ~/px4_ros_ws

colcon build

source install/setup.bash

---

# Run Keyboard Controller

source ~/px4_ros_ws/install/setup.bash

ros2 run drone_keyboard keyboard_control

---

# Keyboard Layout

Movement

W → Forward

S → Backward

A → Left

D → Right

Altitude

I → Up

K → Down

Rotation

J → Rotate Left

L → Rotate Right

Flight Commands

T → Takeoff

G → Land

Space → Emergency Stop

ESC → Exit Program

---

# Flight Procedure

1. Start PX4

2. Start Gazebo

3. Start DDS Agent

4. Start Keyboard Controller

5. Press

T

Drone enters Offboard Mode.

Drone Arms.

Drone Takes Off.

6. Fly using

W A S D

7. Land

G

---

# ROS2 Topics Used

Published

/fmu/in/offboard_control_mode

/fmu/in/trajectory_setpoint

/fmu/in/vehicle_command

Subscribed

/fmu/out/vehicle_status

/fmu/out/vehicle_local_position

---

# Offboard Control

The controller continuously publishes

OffboardControlMode

TrajectorySetpoint

VehicleCommand

at a fixed rate.

PX4 requires continuous setpoints.

If publishing stops for more than 0.5 seconds,

PX4 automatically exits Offboard mode for safety.

---

# Safety Features

Automatic heartbeat publishing

Failsafe compatible

Emergency stop key

Controlled landing

Offboard timeout protection

---

# Example Flight

Start controller

ros2 run drone_keyboard keyboard_control

Takeoff

T

Move Forward

W

Move Left

A

Move Right

D

Move Back

S

Increase Height

I

Decrease Height

K

Rotate Left

J

Rotate Right

L

Land

G

---

# Applications

Indoor drone testing

Autonomous mapping

Camera calibration

LiDAR mapping

SLAM testing

Obstacle avoidance

Warehouse inspection

Research projects

# Live Camera Streaming using ROS2 and Gazebo Harmonic

---

# Overview

The simulated PX4 drone is equipped with an RGB camera (IMX214). The camera images are published by Gazebo and bridged into ROS2 using `ros_gz_bridge`.

The live image stream can then be viewed using `rqt_image_view` or used by computer vision, SLAM, or AI applications.

---

# Camera Architecture

```
                  Gazebo Harmonic
                        │
                        │
                IMX214 RGB Camera
                        │
                        ▼
               Gazebo Image Topic
                        │
                        ▼
                 ros_gz_bridge
                        │
                        ▼
                 ROS2 Image Topic
                        │
             ┌──────────┴──────────┐
             │                     │
             ▼                     ▼
      rqt_image_view        OpenCV / AI
```

---

# Camera Model

Drone

```
x500_depth
```

Sensor

```
IMX214
```

---

# Gazebo Topics

Image

```
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

Camera Info

```
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info
```

---

# ROS2 Bridge

Open a new terminal

```bash
source /opt/ros/jazzy/setup.bash

ros2 run ros_gz_bridge parameter_bridge \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image@sensor_msgs/msg/Image@gz.msgs.Image \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info@sensor_msgs/msg/CameraInfo@gz.msgs.CameraInfo
```

Leave this terminal running.

---

# Verify Camera Topics

```bash
source /opt/ros/jazzy/setup.bash

ros2 topic list | grep IMX214
```

Expected

```
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image

/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info
```

---

# Verify Image Rate

```bash
ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

Expected

```
6–20 Hz
```

---

# Verify Camera Information

```bash
ros2 topic echo \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info --once
```

You should receive

- Camera Matrix
- Distortion Parameters
- Frame ID
- Timestamp

---

# Verify Image Data

```bash
ros2 topic echo \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image --once
```

Expected

```
height: 480

width: 640

encoding: rgb8
```

---

# Open Live Camera

```bash
source /opt/ros/jazzy/setup.bash

rqt_image_view
```

Select

```
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

The live camera feed from the drone should now be displayed.

---

# Camera Specifications

Resolution

```
640 × 480
```

Encoding

```
rgb8
```

Frame Rate

```
≈ 7–20 FPS
```

---

# Why Resolution Was Reduced

Initially the camera operated at

```
1920 × 1080
```

This caused

- Slow image publishing
- RTAB-Map synchronization issues
- High CPU usage
- Low frame rate

The resolution was reduced to

```
640 × 480
```

Benefits

✔ Higher frame rate

✔ Lower CPU usage

✔ Better synchronization

✔ Improved SLAM performance

---

# Camera Optimization

The camera model was modified in

```
~/drone_sim/PX4-Autopilot/Tools/simulation/gz/models/OakD-Lite/model.sdf
```

Image resolution

```xml
<image>
    <width>640</width>
    <height>480</height>
</image>
```

Camera update rate

```xml
<update_rate>20</update_rate>
```

---

# ROS2 Commands

Show Image Topics

```bash
ros2 topic list | grep image
```

Show Camera Info

```bash
ros2 topic list | grep camera_info
```

Image Frequency

```bash
ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

Image Type

```bash
ros2 topic type \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

Camera Info Type

```bash
ros2 topic type \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/camera_info
```

---

# Troubleshooting

## No Image Appears

Check the bridge

```bash
ros2 node list
```

Verify

```
ros_gz_bridge
```

is running.

---

## No Image Topic

Verify Gazebo

```bash
gz topic -l | grep IMX214
```

---

## Low Frame Rate

Reduce camera resolution.

Lower update rate.

Close unused Gazebo windows.

Disable unnecessary visualizations.

---

## rqt_image_view Blank

Check

```bash
ros2 topic hz \
/world/default/model/x500_depth_0/link/camera_link/sensor/IMX214/image
```

If the topic rate is 0 Hz, restart the bridge.

---

# Applications

This RGB camera can be used for

- Object Detection
- YOLO
- OpenCV
- Visual SLAM
- RTAB-Map
- AprilTag Detection
- QR Code Detection
- AI Navigation
- Autonomous Landing
- Warehouse Inspection

---

# Project Status

✔ PX4 Connected

✔ Gazebo Running

✔ ROS2 Bridge Working

✔ Camera Streaming Successfully

✔ Live Image Display in rqt_image_view

✔ Ready for Computer Vision

✔ Ready for Autonomous Navigation

  

https://github.com/user-attachments/assets/1a017430-0638-4927-b911-b941bb274406

      
