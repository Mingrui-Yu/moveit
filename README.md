# Mingrui's moveit.


## 从源码安装 MoveIt

参考 [MoveIt Document Getting Started](https://ros-planning.github.io/moveit_tutorials/doc/getting_started/getting_started.html):

```
# 准备工作
rosdep update
sudo apt update
sudo apt dist-upgrade

sudo apt install ros-noetic-catkin python3-catkin-tools python3-osrf-pycommon
sudo apt install python3-wstool

# 工作空间
cd <WORKSPACE>/src

# 下载全部 moveit 其他没有修改的包
wstool init .
wstool merge -t . https://raw.githubusercontent.com/ros-planning/moveit/master/moveit.rosinstall
wstool remove moveit_tutorials
wstool remove moveit
wstool remove panda_moveit_config
wstool update -t .
```

安装修改后的 moveit 系列包：

```
cd <WORKSPACE>/src

# 下载 moveit
git clone https://github.com/THU-DA-Robotics/moveit.git -b mingrui-noetic
```

安装其他依赖：

```
cd <WORK_PACE>/src
rosdep install -y --from-paths . --ignore-src --rosdistro noetic
```

编译：
```
cd <WORK_PACE>
catkin_make -j8
```

## Note
### 关于 distance_field 的 resolution
在 moveit_core/collision_distance_field/collision_env_distance_field.h 中定义了默认的 resolution（0.02）和 size，目前这些参数不太方便从外面传入。如需修改，还是直接在源文件里改吧。

## Change Log

### 2022.08.08

Problem: 当环境障碍物形式为octomap时，原代码没有正确地将octree转换为points，添加进planning_scene维护的distance_field里面。导致使用distance_field得到的距离障碍物的distance和gradient都是错误的。

Modification:
* 主要修改了 moveit_core/collision_distance_field/collision_env_distance_field.cpp中的updateDistanceObject()中将octree转为points的方式。
* 涉及到的文件包括：
    * moveit_core/collision_distance_field
        * collision_distance_field_types.h (cpp)
        * collision_env_distance_field.h (cpp)
    * moveit_core/distance_field
        * distance_field.h (cpp)

















---



<img src="https://moveit.ros.org/assets/logo/moveit_logo-black.png" alt="MoveIt Logo" width="200"/>

The [MoveIt Motion Planning Framework for ROS](http://moveit.ros.org). For the ROS 2 repository see [MoveIt 2](https://github.com/ros-planning/moveit2).

*Easy-to-use open source robotics manipulation platform for developing commercial applications, prototyping designs, and benchmarking algorithms.*

- [Overview of MoveIt](https://moveit.ros.org)
- [Installation Instructions](https://moveit.ros.org/install/)
- [Documentation](https://moveit.ros.org/documentation/source-code-api/)
- [Get Involved](https://moveit.ros.org/about/get_involved/)

## Branches Policy

- We develop latest features on `master`.
- The `*-devel` branches correspond to released and stable versions of MoveIt for specific distributions of ROS. `noetic-devel` is synced to `master` currently.
- Bug fixes occasionally get backported to these released versions of MoveIt.

## MoveIt Status

### Continuous Integration

service    | Melodic | Master
---------- | ------- | ------
GitHub | [![Format](https://github.com/ros-planning/moveit/actions/workflows/format.yaml/badge.svg?branch=melodic-devel)](https://github.com/ros-planning/moveit/actions/workflows/format.yaml?query=branch%3Amelodic-devel) [![CI](https://github.com/ros-planning/moveit/actions/workflows/ci.yaml/badge.svg?branch=melodic-devel)](https://github.com/ros-planning/moveit/actions/workflows/ci.yaml?query=branch%3Amelodic-devel) | [![Format](https://github.com/ros-planning/moveit/actions/workflows/format.yaml/badge.svg?branch=master)](https://github.com/ros-planning/moveit/actions/workflows/format.yaml?query=branch%3Amaster) [![CI](https://github.com/ros-planning/moveit/actions/workflows/ci.yaml/badge.svg?branch=master)](https://github.com/ros-planning/moveit/actions/workflows/ci.yaml?query=branch%3Amaster) |
CodeCov | [![codecov](https://codecov.io/gh/ros-planning/moveit/branch/melodic-devel/graph/badge.svg?token=W7uHKcY0ly)](https://codecov.io/gh/ros-planning/moveit) | [![codecov](https://codecov.io/gh/ros-planning/moveit/branch/master/graph/badge.svg?token=W7uHKcY0ly)](https://codecov.io/gh/ros-planning/moveit) |
build farm | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mdev__moveit__ubuntu_bionic_amd64)](https://build.ros.org/job/Mdev__moveit__ubuntu_bionic_amd64) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Ndev__moveit__ubuntu_focal_amd64)](https://build.ros.org/job/Ndev__moveit__ubuntu_focal_amd64/) |
| [![Docker](https://img.shields.io/docker/stars/moveit/moveit.svg)](https://registry.hub.docker.com/moveit/moveit)<br>[![Pulls](https://img.shields.io/docker/pulls/moveit/moveit.svg?maxAge=2592000)](https://hub.docker.com/r/moveit/moveit) | [![automated build](https://img.shields.io/docker/automated/moveit/moveit.svg?maxAge=2592000)](https://hub.docker.com/r/moveit/moveit) | [![docker](https://github.com/ros-planning/moveit/actions/workflows/docker.yaml/badge.svg?branch=master)](https://github.com/ros-planning/moveit/actions/workflows/docker.yaml?query=branch%3Amaster) |

### Licenses

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fros-planning%2Fmoveit.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fros-planning%2Fmoveit?ref=badge_shield)


### ROS Buildfarm

MoveIt Package | Melodic Source | Melodic Debian | Noetic Source | Noetic Debian
-------------- | -------------- | -------------- | ------------- | -------------
**moveit** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit__ubuntu_focal_amd64__binary)
moveit_core | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_core__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_core__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_core__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_core__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_core__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_core__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_core__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_core__ubuntu_focal_amd64__binary)
moveit_kinematics | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_kinematics__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_kinematics__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_kinematics__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_kinematics__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_kinematics__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_kinematics__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_kinematics__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_kinematics__ubuntu_focal_amd64__binary)
**moveit_planners** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_planners__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_planners__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_planners__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_planners__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_planners__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_planners__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_planners__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_planners__ubuntu_focal_amd64__binary)
moveit_planners_ompl | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_planners_ompl__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_planners_ompl__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_planners_ompl__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_planners_ompl__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_planners_ompl__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_planners_ompl__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_planners_ompl__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_planners_ompl__ubuntu_focal_amd64__binary)
moveit_planners_chomp | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_planners_chomp__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_planners_chomp__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_planners_chomp__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_planners_chomp__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_planners_chomp__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_planners_chomp__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_planners_chomp__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_planners_chomp__ubuntu_focal_amd64__binary)
chomp_motion_planner | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__chomp_motion_planner__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__chomp_motion_planner__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__chomp_motion_planner__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__chomp_motion_planner__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__chomp_motion_planner__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__chomp_motion_planner__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__chomp_motion_planner__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__chomp_motion_planner__ubuntu_focal_amd64__binary)
moveit_chomp_optimizer_adapter | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_chomp_optimizer_adapter__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_chomp_optimizer_adapter__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_chomp_optimizer_adapter__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_chomp_optimizer_adapter__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_chomp_optimizer_adapter__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_chomp_optimizer_adapter__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_chomp_optimizer_adapter__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_chomp_optimizer_adapter__ubuntu_focal_amd64__binary)
pilz_industrial_motion_planner | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__pilz_industrial_motion_planner__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__pilz_industrial_motion_planner__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__pilz_industrial_motion_planner__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__pilz_industrial_motion_planner__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__pilz_industrial_motion_planner__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__pilz_industrial_motion_planner__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__pilz_industrial_motion_planner__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__pilz_industrial_motion_planner__ubuntu_focal_amd64__binary)
pilz_industrial_motion_planner_testutils | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__pilz_industrial_motion_planner_testutils__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__pilz_industrial_motion_planner_testutils__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__pilz_industrial_motion_planner_testutils__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__pilz_industrial_motion_planner_testutils__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__pilz_industrial_motion_planner_testutils__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__pilz_industrial_motion_planner_testutils__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__pilz_industrial_motion_planner_testutils__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__pilz_industrial_motion_planner_testutils__ubuntu_focal_amd64__binary)
**moveit_plugins** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_plugins__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_plugins__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_plugins__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_plugins__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_plugins__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_plugins__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_plugins__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_plugins__ubuntu_focal_amd64__binary)
moveit_fake_controller_manager | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_fake_controller_manager__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_fake_controller_manager__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_fake_controller_manager__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_fake_controller_manager__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_fake_controller_manager__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_fake_controller_manager__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_fake_controller_manager__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_fake_controller_manager__ubuntu_focal_amd64__binary)
moveit_simple_controller_manager | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_simple_controller_manager__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_simple_controller_manager__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_simple_controller_manager__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_simple_controller_manager__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_simple_controller_manager__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_simple_controller_manager__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_simple_controller_manager__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_simple_controller_manager__ubuntu_focal_amd64__binary)
moveit_ros_control_interface | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_control_interface__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_control_interface__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_control_interface__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_control_interface__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_control_interface__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_control_interface__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_control_interface__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_control_interface__ubuntu_focal_amd64__binary)
**moveit_ros** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros__ubuntu_focal_amd64__binary)
moveit_ros_planning | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_planning__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_planning__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_planning__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_planning__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_planning__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_planning__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_planning__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_planning__ubuntu_focal_amd64__binary)
moveit_ros_move_group | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_move_group__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_move_group__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_move_group__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_move_group__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_move_group__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_move_group__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_move_group__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_move_group__ubuntu_focal_amd64__binary)
moveit_ros_planning_interface | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_planning_interface__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_planning_interface__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_planning_interface__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_planning_interface__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_planning_interface__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_planning_interface__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_planning_interface__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_planning_interface__ubuntu_focal_amd64__binary)
moveit_ros_benchmarks | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_benchmarks__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_benchmarks__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_benchmarks__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_benchmarks__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_benchmarks__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_benchmarks__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_benchmarks__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_benchmarks__ubuntu_focal_amd64__binary)
moveit_ros_perception | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_perception__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_perception__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_perception__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_perception__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_perception__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_perception__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_perception__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_perception__ubuntu_focal_amd64__binary)
moveit_ros_occupancy_map_monitor | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_occupancy_map_monitor__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_occupancy_map_monitor__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_occupancy_map_monitor__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_occupancy_map_monitor__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_occupancy_map_monitor__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_occupancy_map_monitor__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_occupancy_map_monitor__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_occupancy_map_monitor__ubuntu_focal_amd64__binary)
moveit_ros_manipulation | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_manipulation__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_manipulation__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_manipulation__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_manipulation__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_manipulation__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_manipulation__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_manipulation__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_manipulation__ubuntu_focal_amd64__binary)
moveit_ros_robot_interaction | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_robot_interaction__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_robot_interaction__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_robot_interaction__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_robot_interaction__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_robot_interaction__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_robot_interaction__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_robot_interaction__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_robot_interaction__ubuntu_focal_amd64__binary)
moveit_ros_visualization | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_visualization__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_visualization__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_visualization__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_visualization__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_visualization__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_visualization__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_visualization__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_visualization__ubuntu_focal_amd64__binary)
moveit_ros_warehouse | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_ros_warehouse__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_ros_warehouse__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_ros_warehouse__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_ros_warehouse__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_ros_warehouse__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_ros_warehouse__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_ros_warehouse__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_ros_warehouse__ubuntu_focal_amd64__binary)
moveit_servo | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_servo__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_servo__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_servo__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_servo__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_servo__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_servo__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_servo__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_servo__ubuntu_focal_amd64__binary)
**moveit_runtime** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_runtime__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_runtime__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_runtime__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_runtime__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_runtime__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_runtime__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_runtime__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_runtime__ubuntu_focal_amd64__binary)
moveit_commander | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_commander__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_commander__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_commander__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_commander__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_commander__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_commander__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_commander__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_commander__ubuntu_focal_amd64__binary)
moveit_setup_assistant | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_setup_assistant__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_setup_assistant__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_setup_assistant__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_setup_assistant__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_setup_assistant__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_setup_assistant__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_setup_assistant__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_setup_assistant__ubuntu_focal_amd64__binary)
**moveit_msgs** | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_msgs__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_msgs__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_msgs__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_msgs__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_msgs__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_msgs__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_msgs__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_msgs__ubuntu_focal_amd64__binary)
geometric_shapes | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__geometric_shapes__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__geometric_shapes__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__geometric_shapes__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__geometric_shapes__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__geometric_shapes__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__geometric_shapes__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__geometric_shapes__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__geometric_shapes__ubuntu_focal_amd64__binary)
srdfdom | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__srdfdom__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__srdfdom__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__srdfdom__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__srdfdom__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__srdfdom__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__srdfdom__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__srdfdom__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__srdfdom__ubuntu_focal_amd64__binary)
moveit_visual_tools | [![Build Status](https://build.ros.org/buildStatus/icon?job=Msrc_uB__moveit_visual_tools__ubuntu_bionic__source)](https://build.ros.org/view/Msrc_uB/job/Msrc_uB__moveit_visual_tools__ubuntu_bionic__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Mbin_uB64__moveit_visual_tools__ubuntu_bionic_amd64__binary)](https://build.ros.org/view/Mbin_uB64/job/Mbin_uB64__moveit_visual_tools__ubuntu_bionic_amd64__binary) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_visual_tools__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_visual_tools__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_visual_tools__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_visual_tools__ubuntu_focal_amd64__binary)
moveit_tutorials |  |  | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nsrc_uF__moveit_tutorials__ubuntu_focal__source)](https://build.ros.org/view/Nsrc_uF/job/Nsrc_uF__moveit_tutorials__ubuntu_focal__source) | [![Build Status](https://build.ros.org/buildStatus/icon?job=Nbin_uF64__moveit_tutorials__ubuntu_focal_amd64__binary)](https://build.ros.org/view/Nbin_uF64/job/Nbin_uF64__moveit_tutorials__ubuntu_focal_amd64__binary)


## Stargazers over time

[![Stargazers over time](https://starchart.cc/ros-planning/moveit.svg)](https://starchart.cc/ros-planning/moveit)
