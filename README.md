# IsaacSim UR5 + Robotiq MoveIt + RealSense Integration
 
This repository extends the [IsaacSim UR5 + Robotiq MoveIt Integration](https://github.com/FarkasBalintKaroly/IsaacSim_UR5_RobotIQ_MoveIt) project with an **Intel RealSense D435-style RGBD camera** simulated inside **NVIDIA Isaac Sim**.
 
The camera is mounted fix and publishes RGB and depth image data over ROS 2 topics, which can be visualized in RViz alongside the robot model.
 
---
 
## Features
 
- UR5e robot model with Robotiq 2F-85 gripper
- ROS 2 compatible robot description (URDF/Xacro)
- MoveIt-ready robot configuration
- Isaac Sim USD scene with integrated RGBD camera
- Camera data published over ROS 2:
  - `/rgb/image_raw` — color image stream
  - `/depth/image_raw` — depth image stream
- RViz visualization with RGB and depth image display
 
---
 
## Repository Structure
 
```
.
├── ur5_moveit_realsense.usd
├── README.md
└── src
    ├── ur5e_2f85_description_pkg
    ├── Universal_Robots_ROS2_Description
    ├── Universal_Robots_ROS2_Gazebo_Simulation
    └── ros2_robotiq_gripper
```
 
| Directory / File | Description |
|------------------|-------------|
| `ur5_moveit_realsense.usd` | Isaac Sim stage with UR5 robot and RGBD camera setup |
| `ur5e_2f85_description_pkg` | Robot description package containing URDF, Xacro and launch files |
| `Universal_Robots_ROS2_Description` | Official Universal Robots ROS2 robot description |
| `Universal_Robots_ROS2_Gazebo_Simulation` | Simulation related components |
| `ros2_robotiq_gripper` | Robotiq gripper ROS2 interface |
 
---
 
## Requirements
 
Recommended environment:
 
- **Ubuntu 22.04**
- **ROS 2 Humble**
- **MoveIt 2**
- **NVIDIA Isaac Sim**
- `colcon`
- `rosdep`
 
---
 
## Installation
 
### Create a ROS workspace
 
```bash
mkdir -p ~/isaacsim_ur5_ws/src
cd ~/isaacsim_ur5_ws/src
```
 
Clone the repository:
 
```bash
git clone --recurse-submodules https://github.com/FarkasBalintKaroly/IsaacSim_UR5_RobotIQ_MoveIt_Realsense.git
```
 
If the repository was cloned without submodules:
 
```bash
git submodule update --init --recursive
```
 
### Install dependencies
 
```bash
cd ~/isaacsim_ur5_ws
 
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```
 
### Build the workspace
 
```bash
source /opt/ros/humble/setup.bash
 
cd ~/isaacsim_ur5_ws
colcon build --symlink-install
 
source install/setup.bash
```
 
---
 
## Running the Isaac Sim Scene
 
1. Start NVIDIA Isaac Sim.
2. Open the scene:
 
   ```
   ur5_moveit_realsense.usd
   ```
 
3. Enable required extensions:
   - ROS2 Bridge
   - OmniGraph ROS nodes
 
4. Start simulation.
 
Once the simulation is running, the following topics will be published:
 
```
/joint_states
/tf
/rgb/image_raw
/depth/image_raw
```
 
---
 
## Visualizing in RViz
 
Launch the robot model with RViz:
 
```bash
ros2 launch ur5_moveit_config demo.launch.py
```
 
This will start:
 
- `robot_state_publisher`
- RViz visualization
- URDF/Xacro robot model
 
To display the camera streams in RViz, add the following displays manually or via a preconfigured `.rviz` file:
 
- **Image** display → Topic: `/rgb/image_raw`
- **Image** display → Topic: `/depth/image_raw`
 
---
 
## MoveIt Integration
 
Typical workflow:
 
1. Isaac Sim runs the robot and camera simulation.
2. ROS 2 nodes publish joint states, TF, and camera data.
3. MoveIt performs motion planning.
4. Planned trajectories are executed in the simulator.
 
---
 
## References
 
- [ROS 2](https://docs.ros.org)
- [MoveIt 2](https://moveit.ros.org)
- [NVIDIA Isaac Sim](https://developer.nvidia.com/isaac-sim)
- [Universal Robots ROS2 packages](https://github.com/UniversalRobots)
- [Intel RealSense](https://www.intelrealsense.com)
