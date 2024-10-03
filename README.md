# 4 DOF Robot Arm

> :warning: **Implementation of ROS2_Control framework to create hardware interface and simulating with gazebo**: Follow the following setup and how-to-run the program

## Installation and Setup
1. Install ROS2 and the suitable gazebo version. Example: ROS2 Jazzy and Gazebo Harmony
2. Install the dependencies:
```
sudo apt install ros-jazzy-gz-ros2-control ros-jazzy-gz-plugin-vendor ros-jazzy-gz-sim-vendor
```
3. Clone this github
4. Build the package using colcon build
---
## How-To-Run
> :warning: Don't forget to source the workspace **on every terminal** with: **source install/setup.bash**

- Viewing with RVIZ2
```
ros2 launch ros2_control_demo_example_9 view_robot.launch.py
```
![Screenshot from 2024-10-03 20-49-35](https://github.com/user-attachments/assets/f4c2ae20-656d-4d6f-a1a1-5eb70923a450)
