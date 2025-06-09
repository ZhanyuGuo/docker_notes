# 3. ROS with GUI using Docker

- [3. ROS with GUI using Docker](#3-ros-with-gui-using-docker)
    - [3.1. ROS2](#31-ros2)
    - [3.2. ROS1](#32-ros1)

## 3.1. ROS2

docker run -it -e DISPLAY=:0 -v /run/desktop/mnt/host/wslg/.X11-unix:/tmp/.X11-unix -v /run/desktop/mnt/host/wslg:/mnt/wslg osrf/ros:jazzy-desktop

## 3.2. ROS1

[ROS Docker Document](https://wiki.ros.org/docker)

