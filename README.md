ROS 2 Event-Driven Architecture: TurtleBot3 Lifecycle Controller

Overview

This repository contains a structured, enterprise-grade ROS 2 architecture for controlling a TurtleBot3 within the Gazebo simulator. Instead of relying on standard nodes, this project implements ROS 2 Lifecycle Nodes (Managed Nodes) to ensure deterministic startup, clean configuration, and fault-tolerant state transitions.

Movement commands are handled asynchronously via Action Servers, making the system highly scalable and production-ready for autonomous mobile robot (AMR) deployments.

Key Features

Lifecycle Management: Clean configuration, activation, and deactivation of hardware interfaces and control logic.

Component-Based Composition: Nodes are written as loadable components to reduce CPU overhead and improve inter-process communication.

Multi-Threaded Executors: Implemented concurrency to handle simultaneous state transitions and action server requests without blocking.

Action Servers: Non-blocking, goal-based trajectory execution for robotic movement.

Modular Deployment: Custom XML-based launch files to orchestrate the bring-up process of the simulation and lifecycle managers.

System Architecture & State Machine

This project utilizes the standard ROS 2 Managed Node state machine:

Unconfigured: Node is instantiated but inactive.

Inactive: Parameters are loaded, memory is allocated, but publishers/action servers are not yet executing.

Active: The robot is actively processing action goals and publishing cmd_vel to Gazebo.

Finalized: Clean memory teardown.

Build and Run Instructions

1. Prerequisites

Ensure you have ROS 2 (Jazzy/Humble) installed along with the standard TurtleBot3 simulation packages:

```
sudo apt install ros-$ROS_DISTRO-turtlebot3-gazebo ros-$ROS_DISTRO-nav2-bringup
```

2. Build the Workspace

Clone this repository into your colcon workspace and build:
```
mkdir -p ~/ros2_ws/src && cd ~/ros2_ws/src
git clone [https://github.com/Abhirup188/TurtleBot3-Lifecycle-Controller.git](https://github.com/Abhirup188/TurtleBot3-Lifecycle-Controller.git)
cd ~/ros2_ws
colcon build --symlink-install
source install/setup.bash
```

3. Launch the System

Launch the Gazebo simulation and the Lifecycle Manager:
```
ros2 launch my_robot_bringup final_project_turtlebot.launch.xml
```

4. Send an Action Goal

Once the node transitions to the Active state, you can send an asynchronous movement command via the Action Server:
```
ros2 action send_goal /turtlebot_move <action_interface_name> "{target_x: 2.0, target_y: 1.0}"
```

Future Work

Integrating SLAM Toolbox for dynamic map generation.

Connecting the Nav2 stack for fully autonomous point-to-point navigation.

Implementing a custom Perception pipeline for obstacle detection.

Developed by Abhirup Chakraborty | NIT Silchar
