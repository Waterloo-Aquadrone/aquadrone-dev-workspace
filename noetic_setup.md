# Setting up virtual machine
Download latest Ubuntu 20.04.4 LTS Desktop image ISO file from: https://releases.ubuntu.com/20.04/

Create New VM in VirtualBox with following settings:
- Linux
- Ubuntu, 64 bit
- 16GB ram
- 128 GB .vdi hard disk, dynamically allocated

Change the Following Settings:
- 8 CPUs
- 100% utilization
- 128 MB Video Memory
- VBoxVGA Graphics Controller
- Disable 3D acceleration
- Storage -> Controller -> Optical Drive -> Pick Ubuntu ISO file (delete the empty one afterwards)

On First Startup:
Select Ubuntu iso file if prompted

Non-Default Installation Settings:
- name: Aquadrone
- computer name: aquadrone-vm
- username: aquadrone
- password: aquadrone
- Log in automatically

Run software updater and update everything. Adjust screen size if needed, and add the terminal to favourites.

# Run these commands in a terminal:
```
sudo apt update
sudo apt upgrade -y
sudo apt install git python3-pip
```

## ROS Noetic setup from http://wiki.ros.org/Installation/Ubuntu
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'  
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654  
sudo apt update  
sudo apt upgrade -y  
sudo apt install ros-noetic-desktop-full  
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential  
sudo rosdep init  
rosdep update  
```

## installing catkin tools
```
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -  
sudo apt-get install python3-catkin-tools  
```

## installing catkin tools python dependancies
```
git clone https://github.com/catkin/catkin_tools.git  
cd catkin_tools  
pip3 install -r requirements.txt  
cd ../  
rm -rf catkin_tools  
```

## Aquadrone setup
```
cd ~  
git clone https://github.com/Waterloo-Aquadrone/aquadrone2020_dev_workspace.git  
cd aquadrone2020_dev_workspace/catkin_ws/src  
git clone https://github.com/Waterloo-Aquadrone/aquadrone2020.git  
cd aquadrone2020    
pip3 install -r pip_requirements.txt  
cd ../
```

## uuv_simulator setup
```
cd ~/aquadrone2020_dev_workspace/catkin_ws/src
git clone https://github.com/tdenewiler/uuv_simulator.git  
cd uuv_simulator  
git checkout --track origin/rosdep-python3  
cd ../../  
rosdep install --from-paths src --ignore-src --rosdistro=noetic -y --skip-keys "gazebo gazebo_msgs gazebo_plugins gazebo_ros gazebo_ros_control gazebo_ros_pkgs"  
cd src/uuv_simulator  
git checkout --track origin/noetic-devel  
cd ../../  
```

## first catkin workspace build
```
source /opt/ros/noetic/setup.bash  
catkin init  
catkin clean -y  
catkin build  
```

```source ~/aquadrone2020_dev_workspace/catkin_ws/devel/setup.bash```

## .bashrc setup
```
echo "" >> ~/.bashrc  
echo "# Aquadrone Setup" >> ~/.bashrc  
echo "source /usr/share/gazebo-11/setup.sh" >> ~/.bashrc  
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc  
echo "source ~/aquadrone2020_dev_workspace/catkin_ws/devel/setup.bash" >> ~/.bashrc  
echo "cd ~/aquadrone2020_dev_workspace/catkin_ws/src/aquadrone2020" >> ~/.bashrc  
echo "" >> ~/.bashrc  
```

## Final Cleanup
```sudo apt autoremove```

# Debugging Issues
- If everything installs correctly and the code seems to be running but Gazebo is just showing a black screen, then try changing the graphics controller in VirtualBox's settings. For me it only worked with the VBoxVGA controller.
- Ongoing issue: when launching Gazebo, the following error appears ```ERROR: cannot launch node of type [message_to_tf/message_to_tf]: message_to_tf```
