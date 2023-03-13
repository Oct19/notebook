# ROS2 Installation

***Follow instructions from ROS2 documentation for specific distribution. Here we use ROS2 Humble for demonstration.***

1. Check locale in Terminal, and make sure you have a locale which supports UTF-8

        locale

2. Setup Sources

        sudo apt install curl gnupg2

        sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg

3. Use Tsinghua ROS2 mirror (Required if in China)

        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

4. Install

        sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
        sudo apt install ros-humble-desktop

## Configuring environment

1. Add sourcing to your shell startup script, so every terminal run ROS2 on startup

        echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

    To undo this, locate your system’s shell startup script and remove the appended source command.

2. Check environment variables

        printenv | grep -i ROS

## Testing

1. To check if ROS2 is installed

    - In one terminal, run

                ros2 run demo_nodes_cpp talker

    - In another terminal, run

                ros2 run demo_nodes_py listener

    - Run Turtlesim

                sudo apt install -y ros-humble-turtlesim
                ros2 run turtlesim turtlesim_node

## Issues

1. command: `ros2 node list` not working

        ros2 daemon stop
        ros2 daemon start
        ros2 daemon status
