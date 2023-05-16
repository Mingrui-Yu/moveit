# Mingrui's moveit.


## Install Mingrui's modified MoveIt from source

(Refer to [MoveIt Document Getting Started](https://ros-planning.github.io/moveit_tutorials/doc/getting_started/getting_started.html)):

```
# preparation
rosdep update
sudo apt update
sudo apt dist-upgrade

sudo apt install ros-noetic-catkin python3-catkin-tools python3-osrf-pycommon
sudo apt install python3-wstool

# workspace
cd <WORKSPACE>/src

# download all other moveit-related packages
wstool init .
wstool merge -t . https://raw.githubusercontent.com/ros-planning/moveit/master/moveit.rosinstall
wstool remove moveit_tutorials
wstool remove moveit
wstool remove panda_moveit_config
wstool update -t .
```

Install the moveit packages in this repository：

```
cd <WORKSPACE>/src

git clone https://github.com/THU-DA-Robotics/moveit.git -b mingrui-noetic
```

Install other dependencies：

```
cd <WORK_PACE>/src
rosdep install -y --from-paths . --ignore-src --rosdistro noetic
```

Build：
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

### 2023.05

Problem：修改了distance_field中将octree转为points的代码。在原实现中，当octomap和distance的resolution一致时，只将octree中node的中心添加进points。但由于octomap每个voxel和distance_field的每个voxel是不对齐的（相差了resolution/2），所以这样会导致distance_field中一个voxel的边界小于octomap中相对应的voxel的边界。

Modification：将octomap的voxel的边界也添加进points中。
