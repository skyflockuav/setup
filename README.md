# Drone Swarm Simulation Environment Setup

## 📘 Index

1. [Introduction](#introduction)
2. [Glossary of Key Terms](#glossary-of-key-terms)
3. [Environment Setup](#environment-setup)
   - [Phase 1: Initial Setup](#phase-1-initial-setup)
   - [Phase 2: Installing PX4, ROS 2, Gazebo Classic, and QGroundControl](#phase-2-installing-px4-ros-2-gazebo-classic-and-qgroundcontrol)
4. [Running the Program](#running-the-program)
5. [Verify Setup](#verify-setup)

---

## 📘 Introduction ✈️🧭🚀
This document captures the detailed journey of setting up a drone swarm simulation environment using ROS 2 Humble, PX4 v1.15.0, Gazebo Classic, QGroundControl, and MAVROS. The goal is to provide a documentary-style blog that serves as a walkthrough and guide for beginners. 🧠🛠️📝

---

## Glossary of Key Terms 🧠📘📌
- **ROS 2**: Think of it as the brain of your robot or drone. It helps you write software to make your robot move, sense, and react. It's built to support large, complex systems through small programs called nodes.
- **PX4**: This is the firmware (like an operating system) that runs on your drone. It handles flight controls, sensor readings, and safety logic.
- **Gazebo Classic**: A virtual world where your drone can fly without leaving your desk. It's a 3D simulator that shows how your drone would act in real life.
- **QGroundControl (QGC)**: A dashboard app for your drone. You use it to see drone telemetry, change settings, and send commands—like takeoff or land.
- **MAVLink**: A language your drone uses to talk to the ground control software or your own code. It's lightweight and fast.
- **MAVROS**: A translator that lets ROS 2 talk to PX4 using MAVLink. It turns drone messages into something your Python or C++ code can understand.
- **Micro XRCE-DDS**: Another way to connect PX4 with ROS 2, often used on small, real hardware devices. It's more complex and usually not needed in simulation.
- **SITL (Software-In-The-Loop)**: A way to test PX4 and your code without a real drone. PX4 thinks it's flying, but it's all simulated.
- **Node**: A small program that does one job, like controlling motors or reading a sensor. You can have many nodes working together.
- **Topic**: A channel that nodes use to send or receive messages. For example, one node might publish GPS data, and another listens to it.
- **Publisher**: A node that sends messages on a topic.
- **Subscriber**: A node that listens to messages on a topic.
- **Package**: A folder that holds your code, launch files, and dependencies—all the files needed to do one job.
- **colcon**: A tool that builds your packages in ROS 2. It makes sure everything compiles and installs correctly.
- **Launch File**: A script that runs multiple nodes or programs at once, so you don’t have to start them one by one.
- **Workspace**: A place where you keep all your ROS 2 code and packages. Usually named something like ros2_ws.
- **source**: A command that sets up your terminal to use ROS 2. You need to run it before launching your code.
- **entry_points**: A way to tell ROS 2 which Python files to run when you type ros2 run your_package your_script.

---

## Environment Setup

### Phase 1: Initial Setup
#### Step 1: Ubuntu Installation (Recommended over WSL)
- **Why not WSL?**
  - WSL has limitations with GUI and graphics support required for Gazebo and QGroundControl.
- **Recommended**: Ubuntu 22.04 (LTS) native installation

#### Creating a Bootable USB with Rufus
1. Download Ubuntu 22.04 ISO from the [Link](https://www.releases.ubuntu.com/22.04/ubuntu-22.04.5-desktop-amd64.iso).
2. Download and launch [Rufus](https://github.com/pbatard/rufus/releases/download/v4.9/rufus-4.9.exe) (Windows).
3. Select USB device and ISO file.
4. Choose a partition scheme (GPT for UEFI).
5. Click Start and Then Press Ok for both the pop-ups, then wait until it's complete.
6. Boot from USB and install Ubuntu.

#### System Requirements
- At least 50 GB free disk space
- 8+ GB RAM (16 GB recommended)
- Dedicated GPU (for Gazebo)

#### Precautions
- Disable Secure Boot for smoother installation.
- Don’t install alongside Windows unless you understand partitioning.
- Always verify the ISO checksum before flashing.

### Phase 2: Installing PX4, ROS 2, Gazebo Classic, and QGroundControl
#### Prerequisite
##### Step 1: System Update and Essential Tools
```bash
cd ~
sudo apt update && sudo apt upgrade -y
sudo apt install snapd
sudo apt install -y \
    curl wget git lsb-release gnupg \
    software-properties-common net-tools \
    ca-certificates unzip zip tar
```

##### Step 2: Install Common Tools & Build Essentials
```bash
sudo apt install -y \
    build-essential \
    cmake \
    ninja-build \
    clang \
    g++ \
    gcc \
    python3-dev \
    python3-pip \
    python3-setuptools \
    python3-numpy \
    python3-empy \
    python3-yaml \
    python3-jinja2 \
    python3-pybind11 \
    libpython3-dev \
    libeigen3-dev \
    libopencv-dev \
    libboost-all-dev \
    libtinyxml2-dev \
    libssl-dev \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev \
    qtbase5-dev \
    libxml2-dev \
    geographiclib-tools
```

##### Step 3: Install Python Packages (PX4 tools)
```bash
pip3 install --user \
    pyserial jinja2 numpy toml pyulog pandas matplotlib empy
```

---

## Running the Program

### Running the Program Present in our Git-Repo
#### Install px4_msgs
```bash
cd ~/project/swarm_drone/src
git clone -b release/1.15 https://github.com/PX4/px4_msgs.git
cd ~/project/swarm_drone
rosdep install --from-paths src --ignore-src -y
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash
```

---

## Verify Setup

### Fix Px4 Simulated world from default to wayland
```bash
cd ~/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds
mv empty.world default.world
mv baylands.world empty.world
```

### Run the below command to run the program
#### For cpp
```bash
cd ~/project/swarm_drone
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch single_drone_flight single_drone_flight_cpp.launch.py
```

#### For python
```bash
cd ~/project/swarm_drone
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch single_drone_flight single_drone_flight.launch.py
```
