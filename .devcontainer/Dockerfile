# Use the official Ubuntu image as a parent image
FROM ubuntu:22.04

ARG USERNAME=SITL
ARG UID=1000
ARG GID=$UID

# Install basic dependencies
RUN apt-get update && apt-get install -y \
    sudo \ 
    curl \
    wget \
    git \
    nano \
    build-essential \
    software-properties-common \
    python3 \
    python3-pip \
    iproute2

# Create symbolic link for python3 as python
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install pexpect
RUN pip3 install pexpect
RUN pip3 install empy==3.3.4
RUN pip3 install MAVProxy

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Create and switch to user
RUN groupadd -g $GID $USERNAME \
    && useradd -lm -u $UID -g $USERNAME -s /bin/bash $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER $USERNAME

# Create workspace so that user own this directory
RUN mkdir -p /home/$USERNAME/src
WORKDIR /home/$USERNAME/

# Clone the ArduPilot repository with submodules
RUN git clone --recurse-submodules https://github.com/ArduPilot/ardupilot.git

# Set environment variables
ENV AUTOTEST_HOME=/home/$USERNAME/src/ardupilot/Tools/autotest
ENV PATH=$AUTOTEST_HOME:$PATH