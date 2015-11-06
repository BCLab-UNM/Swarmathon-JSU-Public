# Swarmathon-ROS

This repository is a ROS (Robot Operating System) controller framework for the Swarmie robots used in the [NASA Swarmathon](http://www.nasaswarmathon.com), a national swarm robotics competition. This particular framework is a ROS implementation of the CPFA (central-place foraging algorithm) developed for [iAnt robot swarms](http://swarms.cs.unm.edu) at the [University of New Mexico](http://www.unm.edu/).

This repository contains:

1. Source code for ROS libraries (i.e. packages) that control different aspects of the Swarmie robot, including localization, mapping, mobility, and obstacle and target detection
2. 3D .STL models for the physical Swarmie build 
3. Bash shell scripts for initializing simulated Swarmies in the Gazebo simulator, as well as physical Swarmies

### Quick Start Installation Guide

Swarmathon-ROS is designed and tested exclusively on Ubuntu 14.04 LTS (Trusty Tahr) and ROS Indigo Igloo. This framework may compile and run correctly under other versions of Ubuntu and ROS, but **NOTE** that these other systems are untested and are therefore not supported at this time.

##### 1. Install ROS Indigo

Follow the detailed instructions for installing ROS Indigo under Ubuntu 14.04 [here](http://wiki.ros.org/indigo/Installation/Ubuntu). We recommend the Desktop-Full installation, which includes the Gazebo 2 simulator.

##### 2. Install additional ROS plugins

Our simulated and physical Swarmies use existing ROS plugins, external to this repo, to facilitate non-linear state estimation through sensor fusion and frame transforms. These plugins are contained in the [robot_localization](http://wiki.ros.org/robot_localization) package, which should be installed using the apt-get package management tool:

```
sudo apt-get install ros-indigo-robot-localization
```

##### 3. Install additional Gazebo plugins

Our simulated Swarmies use existing Gazebo plugins, external to this repo, to replicate sonar, IMU, and GPS sensors. These plugins are contained in the [hector_gazebo_plugins](http://wiki.ros.org/hector_gazebo_plugins) package, which should be installed using the apt-get package management tool:

```
sudo apt-get install ros-indigo-hector-gazebo-plugins
```

##### 4. Install git (if git is already installed, skip to step 5):

```
sudo apt-get install git
```

##### 5. Install Swarmathon-ROS

1. Clone this GitHub repository to your home directory (~):

  ```
  cd ~
  git clone git@github.com:BCLab-UNM/Swarmathon-ROS.git
  ```

2. Rename the downloaded repo so it can be properly identified by ROS and catkin:

  ```
  mv ~/Swarmathon-ROS ~/rover_workspace
  ```

3. Change your current working directory to the root directory of the downloaded repo:

  ```
  cd ~/rover_workspace
  ```

4. Set up GPS submodule:

  ```
  git submodule init
  git submodule update
  ```

5. Compile Swarmathon-ROS as a ROS catkin workspace:

  ```
  catkin_make
  ```
  
6. Update your bash session to automatically source the setup file for Swarmathon-ROS:

  ```
  echo "source ~/rover_workspace/devel/setup.bash" >> ~/.bashrc
  source ~/.bashrc
  ```

7. Update your bash session to automatically export the enviromental variable that stores the location of Gazebo's model files:

  ```
  echo "export GAZEBO_MODEL_PATH=~/rover_workspace/src/rover_misc/gazebo/models" >> ~/.bashrc
  source ~/.bashrc
  ```

8. Editing the QT gui

Install QT IDE

sudo apt-get install qtcreator

sudo apt-get install python-catkin-tools

9. Building the workspace

clean the workspace with "catkin clean -a"

build the workspace with "catkin build"

10. Setup the QT Creator

run "qtcreator &"

Choose open project from the file menu.

Navigate to ~/rover_workspace/src/rqt_rover_gui/

Select CMakeLists.txt

Click yes to creating a .pro file.

QT Creator will ask for the default build path: type in ~/rover_workspace/build

Click configure project.

Click on the projects icon on the left toolbar.

Enter "-DCMAKE_INSTALL_PREFIX=../install -DCATKIN_DEVEL_PREFIX=../devel" in the CMake arguments text box.

Click the edit toolbox icon on the left. Double click CMakeLists.txt. Click the "build now" button to build the project.

Now qtcreator can be used to build the rover_workspace

11. To run the Swarmathon ROS make ~/rover_workspace exectuatable:

cd ~rover_workspace

chmod +x ./run.sh

./run.sh

The GUI will now launch. The run script kills a number of gazebo and ROS processes. Killing these processes is suggested by gazebosim.com as the best way to clean up the gazebo environment at the moment.

This is the first screen of the GUI:

![Alt text](http://swarmathon.cs.unm.edu/img/GUI2.png "Opening Screen")

Click the simulation paramenters tab:

![Alt text](http://swarmathon.cs.unm.edu/img/GUI3.png "Simulation Parameters")

Choose the ground texture, whether this is a preliminary or final round (3 or 6 robots), and the distribution of targets.

Click the "build simulation" button when ready.

The gazebo physics simulator will open.

![Alt text](http://swarmathon.cs.unm.edu/img/GUI4.png "Gazebo Simulator")

Click back to the Swarmathon GUI and select the "sensor display" tab.

![Alt text](http://swarmathon.cs.unm.edu/img/GUI5.png "Gazebo Simulator")

Any active rovers, simulated or real will be displayed in the rover list on the left side.

![Alt text](http://swarmathon.cs.unm.edu/img/GUI6.png "Rover sensor display")

Select a rover to view its sensor outputs. 

![Alt text](http://swarmathon.cs.unm.edu/img/GUI9.png "Rover sensor display")

There are four sensor display frames:

The camera output. This is a rover eyes view of the world.

The ultrasound output is shown as three white bars, one for each ultrasound. The length of the rays indicates the distance to any obects in front of the ultrasound. The distance in meters is displayed in text below these rays. The max distance reported by the ultrasounds is 3m.  

The IMU sensor display consists of a cube where the red face is the bottom of the rover, blue is the top, and the red and blue bars are front and back. The cube is viewed from the top down. The cube is positioned according to the IMU orientation data. For example if the rover flips over the red side will be closes to the observer. Accelerometer data is shown as a 3d vector projected into 2d space ponting towards the sum of the acdcelerations in 3d space.
 
The map view shows the path taken by the currently selected rover. Green is the encoder position date. In simulation the encoder position data comes from the odometry topic being published by skidsteer drive controller plugin. The the real robots it is the encoder output. GPS points are shown as red dots. The EKF is the extended Kalman filter fuses the output of the IMU, GPS, and encoder sensors.