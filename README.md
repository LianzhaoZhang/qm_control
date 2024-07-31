# qm_control

## Overview

The branch ensures the quadruped manipulator's whole-body compliance under actuation saturation. To be specific, in the case of non-saturation, the controller tracks user-set compliance. In case of saturation, it can avoid unsafe behaviors (e.g., slips, oscillations, and overshoots) caused by actuation saturation.

## Video

(Click to see)
[![Watch the video](./docs/cover.jpg)](https://youtu.be/gK7PCxNsuZ8)

***Video Links:*** [YouTube](https://youtu.be/gK7PCxNsuZ8), [Bilibili](https://www.bilibili.com/video/BV1dy421e7PZ) (for China).

## Related Paper  

- T. Zhang, F. Lin, X. Peng, X. Xiong, and Y. Lou. “Whole-body Compliance Control for Quadruped Manipulator with Actuation Saturation of Joint Torque and Ground Friction”, 2024 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), Accepted.

## Install

### Install dependencies

- [OCS2](https://leggedrobotics.github.io/ocs2/installation.html#prerequisites)
- [ROS1-Noetic](http://wiki.ros.org/noetic)

### Clone and Build

```
# Clone
mkdir -p <catkin_ws_name>/src
cd <catkin_ws_name>/src
git clone https://github.com/skywoodsz/qm_control.git
cd ./qm_control
git checkout feature-compliance

# Build
cd <catkin_ws_name>
catkin init
catkin config -DCMAKE_BUILD_TYPE=RelWithDebInfo
catkin build
```

***Notes: Make sure OCS2 is in the environment path.***


## Usage

Launch the simulation with:

```
mon launch qm_gazebo empty_world.launch
```

Load the controller with:

```
mon launch qm_controllers load_controller.launch
```

Start the controller using `rqt_controller_manager` GUI with:

```
rosrun rqt_controller_manager rqt_controller_manager
```

After the manipulator is initialized, commands can be sent with: 

```
# Don't use mon
roslaunch qm_controllers load_qm_target.launch 
# rviz
mon launch qm_controllers rviz.launch
```

[dynamic_reconfigure](http://wiki.ros.org/dynamic_reconfigure) is set to expose parameters to external reconfiguration, such as the torque limit, friction coefficient, stiffness, damping, and so on.

```
rosrun rqt_reconfigure rqt_reconfigure
```



## External force test

-   You can call the service to apply the external force of $[50.0, 0.0, 0.0]^T$ $N$ to the end-effector for 2 seconds.

```
rosservice call /gazebo/apply_body_wrench '{body_name: "qm::ft_sensor", reference_frame: "qm::ft_sensor", wrench: { force: { x: 50.0, y: 0, z: 0 } }, start_time: 0, duration: {secs: 2} }'
```

-   You can call the service to apply the external force of $[50.0, 0.0, 0.0]^T$ $N$ to the base for 2 seconds.

```
rosservice call /gazebo/apply_body_wrench '{body_name: "qm::base", reference_frame: "qm::base", wrench: { force: { x: 50.0, y: 0, z: 0 } }, start_time: 0, duration: {secs: 2} }'
```
