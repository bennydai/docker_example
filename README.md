# Some notes

I personally use Nvidia-Docker right now but I believe if you have the later version of Docker you can mount NVIDIA gpus quite easily.

# Building your Docker Image
Check the bottom of the GUI Dockerfile which requires some environment variables to access the device. Furthermore if you GUI needs OpenGL - NVIDIA requires you to have a particular package installed which is available via apt-get on 18.04 onwards. Hence, why I use ROS Melodic for all visualisation purposes. 

For all Docker images - you will need the libcanberra-gtk-module and some other GUI rendering libraries to generate the GUIs. Generally most Docker Images will have these packages removed to keep them light.

# Running your Docker Image
There are two steps to running GUIs in Docker Images. One is giving sudo privileges to xhost which does the X11 forwarding.

This is indicated in ```run.sh``` with the line ```xhost +local:docker```.

The next step is to provide the variables and volumes to the container so it can run. These are in the docker-compose container instructions in rviz. The main difference between the rovio container and rviz container is that rviz requires OpenGL. The Rovio node just generates an opencv popup window - thus does not require the OpenGL. Both these containers pop up a GUI of somesort.

# Using docker-compose vs docker run
I am lazy - so I use docker-compose to put it in a configurated setup.
