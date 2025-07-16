# ROS2 Humble Setup for Robot Simulation and Control

This guide outlines the steps to set up a ROS 2 Humble environment on Ubuntu 22.04.5 LTS (Jammy) in a virtual machine for robot simulation and control, tailored to your specific setup. Adapted from [IFRA-Cranfield/ros2_SimRealRobotControl Installation Instructions](https://github.com/IFRA-Cranfield/ros2_SimRealRobotControl/blob/humble/instructions/Installation.md).

## Prerequisites
- Ubuntu 22.04.5 LTS (Jammy) installed in a virtual machine
- ROS 2 Humble distribution (`ROS_DISTRO=humble`)

## Installation Steps

### 1. Install Git
```sh
sudo apt install git
git config --global user.name YourUsername
git config --global user.email YourEmail
git config --global color.ui true
git config --global core.editor code --wait
git config --global credential.helper store
```

### 2. Install ROS 2 Humble
Follow the instructions at [ROS2 Humble Tutorials - Installation](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html).
Add to `~/.bashrc`:
```sh
source /opt/ros/humble/setup.bash
```

### 3. Install MoveIt!2
```sh
sudo apt install ros-humble-moveit
```

### 4. Modify move_group_interface.h
Download the improved [move_group_interface_improved.h](https://github.com/IFRA-Cranfield/ros2_SimRealRobotControl/tree/humble/include) to a local directory (e.g., `~/IFRA-Cranfield`). Copy it to the MoveIt system directory:
```sh
sudo cp ~/IFRA-Cranfield/move_group_interface_improved.h /opt/ros/humble/include/moveit/move_group_interface
```
Note: Replace `~/IFRA-Cranfield` with the path where you downloaded the file.

### 5. Create and Configure ROS 2 Workspace
Follow [ROS2 Humble Tutorials - Create a ROS2 Workspace](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html).
Add to `~/.bashrc`:
```sh
source ~/dev_ws/install/local_setup.bash
```

### 6. Install Required ROS 2 Packages
```sh
sudo apt install python3-rosdep
sudo apt install python3-vcstool
sudo apt install python3-colcon-common-extensions
sudo apt install ros-humble-ros2-control
sudo apt install ros-humble-ros2-controllers
sudo apt install ros-humble-gripper-controllers
sudo apt install gazebo
sudo apt install ros-humble-gazebo-ros2-control
sudo apt install ros-humble-gazebo-ros-pkgs
sudo apt install ros-humble-xacro
sudo apt install ros-humble-rmw-cyclonedds-cpp
```
Add to `~/.bashrc`:
```sh
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```

#### Downgrade Gazebo-ROS2-Control to 0.4.6
```sh
sudo apt remove ros-humble-gazebo-ros2-control
cd ~/dev_ws/src
git clone https://github.com/ros-controls/gazebo_ros2_control.git
cd gazebo_ros2_control
git reset --hard 9a3736c
cd ~/dev_ws
colcon build
```

### 7. Install ABB Driver for ROS 2
```sh
mkdir -p ~/dev_ws/src/ABBDriver
cd ~/dev_ws/src/ABBDriver
git clone https://github.com/PickNikRobotics/abb_ros2.git -b rolling
sudo rosdep init
rosdep update
vcs import < abb_ros2/abb.repos
rosdep install -r --from-paths . --ignore-src --rosdistro $ROS_DISTRO -y
cd ~/dev_ws
colcon build
```

### 8. Install Universal Robots ROS 2 Driver
```sh
sudo apt-get install ros-humble-ur
```

### 9. Install IFRA-Cranfield ROS 2 Packages
```sh
cd ~/dev_ws/src
git clone https://github.com/IFRA-Cranfield/IFRA_LinkAttacher.git
cd ~/dev_ws
colcon build

cd ~/dev_ws/src
git clone https://github.com/IFRA-Cranfield/IFRA_ObjectPose.git
cd ~/dev_ws
colcon build

cd ~/dev_ws/src
git clone https://github.com/IFRA-Cranfield/IFRA_LinkPose.git
cd ~/dev_ws
colcon build

cd ~/dev_ws/src
git clone https://github.com/IFRA-Cranfield/ros2_RobotiqGripper.git
cd ~/dev_ws
colcon build
```

### 10. Install ros2_SimRealRobotControl
```sh
cd ~/dev_ws/src
git clone https://github.com/IFRA-Cranfield/ros2_SimRealRobotControl
cd ~/dev_ws
colcon build
```

## Notes
- Ensure all commands are executed in a terminal.
- Source `~/.bashrc` after modifications: `source ~/.bashrc`.
- Verify setup: `printenv | grep ROS_DISTRO` should return `ROS_DISTRO=humble`.
