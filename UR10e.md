# UR10e Ubuntu 22.04 setup

* Install Ubuntu 22.04 with ROS 2 Humble and Moveit2
* Additional ros2 modules to install

  ```
  sudo apt install ros-humble-ros2-control ros-humble-ros2-controllers ros2controlcli -y
  ```

  ```bash
  sudo apt-get install ros-${ROS_DISTRO}-ros2controlcli -y
  ```
* Setup workspace

  ```bash
  # you may swap ur10e with other versions
  export COLCON_WS=~/ur10e_ws
  mkdir -p $COLCON_WS/src 
  cd $COLCON_WS/src
  git clone https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver.git -b humble
  git clone https://github.com/UniversalRobots/Universal_Robots_ROS2_Description.git -b humble
  sudo apt install python3-pip
  sudo pip3 install rosdepc
  sudo rosdepc init 
  sudo rosdepc update
  cd ..
  rosdepc install --from-paths src -y -r

  # Compile (if compile failed, remove work folder and redo)
  colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
  #source the install file, make sure the swap ur10e with your workspace name
  echo "source ~/ur10e_ws/install/local_setup.sh">>~/.bashrc
  ```

## Option 1: Use with UR Simulator

1. Install docker

   ```bash
   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   #manage docker as non-root user
   sudo groupadd docker
   sudo usermod -aG docker $USER
   newgrp docker
   sudo vim /etc/docker/daemon.json
   ```
1. Set up docker proxy since it has been banned in China: Check Ubuntu network proxy, it should be something like `127.0.0.1:7897`. Make sure your vpn proxy (such as clash verge) also use the same port(in this case 7897). Find your docker settings file `daemon.json ` and add the following lines. (Note that http and https string are the same)

   ```json
   {
   "proxies": {
   "http-proxy": "http://127.0.0.1:7897",
   "https-proxy": "http://127.0.0.1:7897",
   "no-proxy": "*.test.example.com,.example.org,127.0.0.0/8"
   }
   }
   ```
1. Test docker connection

   ```
   sudo systemctl restart docker
   docker run hello-world
   ```
1. Open UR Sim

   ```bash
   docker pull universalrobots/ursim_e-series
   # swap ur10e with other versions
   ros2 run ur_client_library start_ursim.sh -m ur10e
   # Open the link in terminal log in web browser
   ```
1. In UR sim, create a new program: `New->Program->URCaps->External Control->Save>Name as 'Remote Control'`
1. Run the newly created remote control program `Click on the "[>]" button on the bottom left corner, select remote control program`
1. Use Moveit 2 to move URSim

   ```bash
   ros2 launch ur_robot_driver ur_control.launch.py ur_type:=ur10e robot_ip:=192.168.56.101 launch_rviz:=false
   #New terminal
   ros2 launch ur_moveit_config ur_moveit.launch.py ur_type:=ur10e robot_ip:=192.168.56.101 launch_rviz:=true
   ```
1. In Moveit2, you can click on arrows to drag the robotic arm's end effector. In the motion planning windows, click `Plan&Execute`, the robot in UR Sim should move at the same time. Note: all Move command in UR Sim will automaticlly stop the `remote control` program. In this case UR Sim can no longer be controlled remotely, re-run the program to establish remote control.

## Option 2: Connect to UR real robot

1. Connect to robot via ethernet, and setup host ip address the same as the `HOST IP` on UR teaching pendant
2. ping UR ip to check connection `ping 192.168.1.24`
3. switch UR to remote mode using teaching pendent (refer to step 5-6 in Option 2)
4. Use Moveit 2 to move UR robot

   ```bash
   ros2 launch ur_robot_driver ur_control.launch.py ur_type:=ur10e robot_ip:=192.168.1.24 launch_rviz:=false
   #New terminal
   ros2 launch ur_moveit_config ur_moveit.launch.py ur_type:=ur10e robot_ip:=192.168.1.24 launch_rviz:=true

   #Visualize what is going on...
   ros2 run rqt_graph rqt_graph
   ```
