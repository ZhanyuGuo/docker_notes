# Use the official ROS Noetic base image
FROM osrf/ros:noetic-desktop-full

SHELL ["/bin/bash", "-c"]

RUN apt-get update --fix-missing \
    && apt-get -y --no-install-recommends install \
    git \
    ros-noetic-octomap* \
    && echo "source /opt/ros/noetic/setup.bash" >> /root/.bashrc
