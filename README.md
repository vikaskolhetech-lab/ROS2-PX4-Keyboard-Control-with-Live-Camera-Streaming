# ROS2-PX4-Keyboard-Control-with-Live-Camera-Streaming
A complete ROS2 Jazzy and PX4 SITL project demonstrating keyboard control of an X500 drone in Gazebo Harmonic with real-time camera streaming using ROS2 image transport. The project includes Micro XRCE-DDS communication, PX4 Offboard Control, Gazebo simulation, ROS2 image bridge, and live camera visualization using rqt_image_view.
ROS2 Autonomous Drone Simulation using PX4 + Gazebo Harmonic
# ROS2 PX4 Autonomous Drone Simulation
### Keyboard Control | Live Camera Streaming | LiDAR Mapping | Gazebo Harmonic | Ubuntu 24.04

---

# Project Overview

This project demonstrates a complete autonomous drone simulation using:

- Ubuntu 24.04
- ROS2 Jazzy
- Gazebo Harmonic
- PX4 Autopilot
- Micro XRCE DDS
- Keyboard Teleoperation
- Live RGB Camera Streaming
- LiDAR Mapping using SLAM Toolbox

The project begins with manual keyboard control and later extends to autonomous mapping using a simulated LiDAR sensor.

The drone can

✔ Spawn inside Gazebo

✔ Arm

✔ Takeoff

✔ Fly using keyboard

✔ Stream live RGB camera

✔ Publish LiDAR scans

✔ Generate 2D occupancy maps

---

# Hardware Used

Laptop

Ubuntu 24.04 LTS

Minimum 16 GB RAM

NVIDIA GPU Recommended

---

# Software Stack

Ubuntu 24.04

ROS2 Jazzy

Gazebo Harmonic

PX4 Autopilot

Micro XRCE DDS Agent

ros_gz_bridge

SLAM Toolbox

RViz2

rqt_image_view

---

# Folder Structure

```
drone_sim/
│
├── PX4-Autopilot/
│
├── px4_ros_ws/
│   ├── src/
│   │
│   ├── drone_keyboard/
│   ├── drone_controller/
│   ├── drone_control/
│   ├── px4_msgs/
│   └── px4_ros_com/
│
└── README.md
```

---

# Install Ubuntu

Ubuntu 24.04 LTS

---

# Install ROS2 Jazzy

```bash
sudo apt update
sudo apt install ros-jazzy-desktop -y
```

Add ROS environment

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

# Install Gazebo Harmonic

```bash
sudo apt install gz-harmonic -y
```

Verify

```bash
gz sim
```

---

# Install PX4

Clone repository

```bash
mkdir -p ~/drone_sim
cd ~/drone_sim

git clone https://github.com/PX4/PX4-Autopilot.git

cd PX4-Autopilot
```

Install dependencies

```bash
bash ./Tools/setup/ubuntu.sh
```

Restart terminal

---

# Build PX4

```bash
cd ~/drone_sim/PX4-Autopilot

make px4_sitl
```

---

# Create ROS Workspace

```bash
mkdir -p ~/px4_ros_ws/src

cd ~/px4_ros_ws/src
```

Clone PX4 ROS packages

```bash
git clone https://github.com/PX4/px4_msgs.git

git clone https://github.com/PX4/px4_ros_com.git
```

Clone custom packages

```
drone_keyboard

drone_control

drone_controller
```

---

# Build Workspace

```bash
cd ~/px4_ros_ws

source /opt/ros/jazzy/setup.bash

colcon build
```

After successful build

```bash
source install/setup.bash
```

---

# Install DDS Agent

```bash
sudo apt install ros-jazzy-micro-xrce-dds-agent
```

---

# Install Gazebo Bridge

```bash
sudo apt install ros-jazzy-ros-gz-bridge
```

---

# Install Image Viewer

```bash
sudo apt install ros-jazzy-rqt-image-view
```

---

# Install SLAM Toolbox

```bash
sudo apt install ros-jazzy-slam-toolbox
```

---

# Verify Installation

ROS

```bash
ros2 doctor
```

Gazebo

```bash
gz sim
```

PX4

```bash
make px4_sitl
```

DDS

```bash
MicroXRCEAgent udp4 -p 8888
```

Everything is now installed.

The next section explains the complete startup procedure for every terminal required to fly the drone.
