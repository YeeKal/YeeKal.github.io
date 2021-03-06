---
title: moveit source installation
categories: ros
tags: ros
date: 2018-07-18
---

## source installation with moveit

reference:

- [official](http://moveit.ros.org/install/source/)
- [qiu-zhihu](https://www.zhihu.com/question/55861914)

version corresponding:

- moveit-melodic with ompl-master
- moveit-kinetic with ompl-1.4.0
- sudo apt-get install ros-kinetic-trac-ik-lib
- sudo apt-get remove ros-kinetic-ompl

- install ros
- [moveit source installation](http://moveit.ros.org/install/source/)
    - prerequisites
- [building from source](http://moveit.ros.org/install/source/dependencies/)

- uninstall moveit: sudo apt-get remove ros-kinetic-moveit-*

## errors

1. [Removed EuclideanProjection, changed to Eigen vector.](https://github.com/ros-planning/moveit/pull/903/files)
2. libompl.so: copied from "/usr/local/lib" to destination.
3. "stack smashing detected" when collision checking with fcl after install openrave with fcl source installation. Checkout to the kinect version(fcl-0.5) and make install, then rebuild moveit.

The initial libfcl.so is installed in '/usr/lib/x86_64-linux-gnu', however the new built library is installed in '/usr/local/lib'， so the 'devel' and 'build' directory should be deleted before re-build moveit.

## add a new planner

**Modify MoveIt! Source**

In your MoveIt! directory, modify src/moveit_planners/ompl/ompl_interface/src/planning_context_manager.cpp. Include the header file that defines your planner at the top and register your planner in the registerDefaultPlanners() function as the existing ones are

**Install MoveIt! from Source**

Finish the last 2 steps of the MoveIt! installation (catkin_make and source the install)

If you **did not overwrite the existing OMPL**  in your ros directory, you will have to set environment variables to tell MoveIt! which version of OMPL to use

You can set: OMPL_PREFIX = [local install directory]

append OMPL_PREFIX to CMAKE_PREFIX_PATH

**Modify ompl_planning.yaml**

If you've already generated MoveIt! config files for your robot, modify /config/ompl_planning.yaml in YOURROBOT_moveit_config to include the option for the new planner.

**re-build**

for the older package which uses moveit, clean the build files(delete build||devel) and re-build this package.

```
catkin build //rebuild
 catkin config --cmake-args -DCMAKE_BUILD_TYPE=Debug //set build model debug/release
```


## reference

[ Quadcopter path planning using ompl in ros](https://github.com/ayushgaud/path_planning)
