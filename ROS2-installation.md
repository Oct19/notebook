# ROS2 Installation

***Follow instructions from ROS2 documentation for specific distribution. Here we use ROS2 Humble for demonstration.***

1. Check locale in Terminal, and make sure you have a locale which supports UTF-8

   ```bash
   locale
   ```
2. Setup Sources

   ```bash
   sudo apt install curl gnupg2sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
   ```
3. Use Tsinghua ROS2 mirror (Required if in China)

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
   ```
4. Update

   ```bash
   sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
   sudo apt install ros-humble-desktop
   ```
5. Install ROS desktop,

   ```
   # Option 1: Standard install
   sudo apt install ros-humble-desktop
   # Option 2: Install bare bones ROS, without GUI tools
   sudo apt install ros-humble-ros-base
   sudo apt install ros-dev-tools
   ```
6. Add sourcing to your shell startup script, so every terminal run ROS2 on startup

   ```
   echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
   ```

   After adding script, restart terminal to take effect
7. Check environment variables

   printenv | grep -i ROS
8. Run ROS example

   ``ros2 run demo_nodes_cpp talker``

## Issues

* command: `ros2 node list` not working

  ```
  ros2 daemon stop
  ros2 daemon start
  ros2 daemon status
  ```
* setuptools, libstdc++, GLIBCXX version issue: if you have conda installed, make sure both conda and pip are both on the correct version

## Reference

* [ROS2 tutorial by Robotics Back-End](https://youtu.be/Gg25GfA456o)
