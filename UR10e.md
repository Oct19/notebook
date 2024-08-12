# UR10e Ubuntu 22.04 setup

* Install ros2 humble
* Install docker

  ```bash
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  #manage docker as non-root user
  sudo groupadd docker
  sudo usermod -aG docker $USER
  newgrp docker
  # test
  docker run hello-world
  ```
* Set up docker proxy since it has been banned in China: Check network proxy, it should be something like `127.0.0.1:7897`. Make sure your vpn proxy also use the same port(in this case 7897)

  ```bash
  sudo vim /etc/docker/daemon.json

  ### Edit the file, note that http and https string are the same
  {
    "proxies": {
      "http-proxy": "http://127.0.0.1:7897",
      "https-proxy": "http://127.0.0.1:7897",
      "no-proxy": "*.test.example.com,.example.org,127.0.0.0/8"
    }
  }

  ### save and exit

  sudo systemctl restart docker
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
  rosdep install

  # Compile
  colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
  #source the install file, make sure the swap ur10e with your workspace name
  echo "source ~/ur5_ws/install/local_setup.sh">>~/.bashrc

  # launch
  ros2 launch ur_moveit_config ur_moveit.launch.py ur_type:=ur5e launch_rviz:=true
  ```
* URSim

  ```bash
  docker pull universalrobots/ursim_e-series
  # swap ur10e with other versions
  ros2 run ur_client_library start_ursim.sh -m ur10e

  ```
* In new terminal, run rviz

```bash
ros2 launch ur_robot_driver ur_control.launch.py ur_type:=ur10e robot_ip:=192.168.56.101 launch_rviz:=true
```
