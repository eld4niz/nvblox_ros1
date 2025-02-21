FROM nvidia/cuda:11.8.0-devel-ubuntu20.04

# Set timezone properly
ENV TZ="Europe/Warsaw"

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata && \
    ln -fs /usr/share/zoneinfo/Europe/Warsaw /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata

# Install required packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr gnupg2 curl

# Setup ROS repository securely
RUN curl -fsSL http://packages.ros.org/ros.key | gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros/ubuntu focal main" | tee /etc/apt/sources.list.d/ros1-latest.list

# Set environment variables
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV ROS_DISTRO=noetic

# Install ROS
RUN apt-get update && apt-get install -y --no-install-recommends ros-noetic-desktop-full && \
    rm -rf /var/lib/apt/lists/*

# Copy scripts and set permissions
COPY scripts/ /root/scripts/
WORKDIR /root/
RUN chmod +x /root/scripts/*.sh

# Environment variables
ENV NVBLOX_DEP_WS=/root/nvblox_dep_ws
ENV SCRIPTS_PATH=/root/scripts

# Run installation scripts
RUN /root/scripts/install_sys_deps.sh && \
    /root/scripts/install_ros_deps.sh && \
    /root/scripts/build_ros.sh
