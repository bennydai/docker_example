# Pull and build on top of the Official ROS Docker Image 
FROM osrf/ros:kinetic-desktop-full

# Install system requirements that is needed to build later packages
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
	software-properties-common \
	&& apt-get autoremove -y \
	&& rm -r /var/lib/apt/lists/*

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]

# Installing Software for Rendering GUIs and Editing
RUN apt-get update && apt-get install -y vim git

RUN apt-get install -y --reinstall libcanberra-gtk-module libsm6 libxext6 libxrender-dev

# Turn on colourful terminal shell 
ENV TERM xterm-256color

WORKDIR /root/

# Upgrade Python Packages, cmake and catkin tools
RUN apt-get install -y python-pip
RUN pip install --upgrade pip
RUN pip install cmake --upgrade
RUN pip install catkin-tools
RUN pip install catkin_pkg
RUN apt-get install -y python-catkin-tools
RUN rosdep update

# Installing pre-requisites for ROS packages
# Setting up ROS catkin_ws
RUN mkdir -p /root/catkin_ws/src

WORKDIR /root/catkin_ws

RUN /bin/bash -c "/ros_entrypoint.sh; catkin config \
	--init --mkdirs --extend /opt/ros/kinetic --merge-devel \
	--cmake-args -DCMAKE_BUILD_TYPE=Release"

# Installing Pre-requisite ROS software
WORKDIR /root/catkin_ws/src

# Kindr
RUN git clone https://github.com/anybotics/kindr.git
RUN /bin/bash -c "source /ros_entrypoint.sh; catkin build -w ~/catkin_ws kindr"

# Python GUI tools
RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections && \
	apt-get install -y python-tk python-gtk2 python-gtk2-dev

RUN rosdep update
# Rovio
RUN git clone https://github.com/ethz-asl/rovio.git

WORKDIR /root/catkin_ws/src/rovio

RUN git submodule update --init --recursive

RUN /bin/bash -c "source /ros_entrypoint.sh; catkin build rovio --cmake-args \
	-DCMAKE_BUILD_TYPE=Release"

RUN apt-get update && apt-get -y autoremove && rm -r /var/lib/apt/lists/*

RUN echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc

# Reseting back
WORKDIR /root
SHELL ["/bin/bash", "-c"]
RUN . /root/.bashrc
