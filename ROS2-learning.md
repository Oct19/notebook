# ROS2-learning

For installation, check `ROS2-installation.md`

## Your fisrt node

* Create a folder as workspace

  ``mkdir ~/ros2_ws``
* Create src folder

  ``cd ~/ros2-ws mkdir src``
* Create a python package

  ```
  cd src 
  ros2 pkg create --build-type ament_python my_robot_controller --dependencies rclpy
  ```
* Create an excutable python file

  ```
  cd my_robot_controller/my_robot_controller 
  touch my_first_node.py 
  chmod +x my_first_node.py
  ```
* Edit python file

  ```python
  #!/usr/bin/env python3
  import rclpy
  from rclpy.node import Node

  class MyNode(Node):

      def __init__(self):
          super().__init__("first_node")
          self.counter_ = 0
          self.create_timer(1.0, self.timer_callback)

      def timer_callback(self):
          self.get_logger().info("Hello" + str(self.counter_))

  def main(args=None):
      #start ros communication
      rclpy.init(args=args)

      node = MyNode()

      #keep node alive
      rclpy.spin(node)

      #distroy node and stop ros communication
      rclpy.shutdown

  if __name__ == '__main__':
      main()
  ```
* Edit `ros2_ws/src/my_robot_controller/setup.py`

  ```python
  entry_points={
          'console_scripts': [
              #excutable = package_name.file_name:function_name
              "test_node = my_robot_controller.my_first_node:main"
          ],
      },
  ```
* Build

  ```
  cd ~/ros2_ws
  colcon build --symlink-install
  ```

  symlink: after python file change, no need to rebuild to apply changes

  If has warning about setuptools, you may need to downgrade setuptools. If you have both conda and pip, ensure the one that is actually been used is downgraded to the correct version

  ```
  pip install setuptools==58.2.0
  ```
* Edit `~/.bashrc`

  ```
  source /opt/ros/humble/setup.bash
  source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
  source ~/ros2_ws/install/setup.bash
  ```
* Run

  ```
  source ~/.bashrc
  ros2 run my_robot_controller test_node
  ```
* Remove a package

  ``rm -rf src/<package_name> rm -rf build/ install/ log/ colcon build``
  After remove a package or deleting related folders, you might encounter warning `...AMENT_PREFIX_PATH doesn't exist`, try build again. If issue persists, edit `AMENT_PREFIX_PATH`

  ```
  export AMENT_PREFIX_PATH = "/home/dingkun/ros2_ws/install/my_robot_controller:/opt/ros/humble"
  ```

## Learn Node, Topic, Service with Turtlesim

* How to install a package:

  ```
  sudo apt install ros-humble-turtlesim
  # name format: ros-distribution-package-name
  ```
* Run turtlesim_node from the package

  ```
  ros2 run turtlesim turtlesim_node
  # use tab to auto complete package name and node name
  ```
* In another terminal, run these one by one

  ```
  # check running nodes
  ros2 node list
  # check running topics
  ros2 topic list
  # check running services
  ros2 service list
  # visualize
  rqt_graph
  ```

### Topic

* To check topic info

  ```
  ros2 topic info /turtle1/cmd_vel
  # return Type: geometry_msgs/msg/Twist
  # Publisher count: 0
  # Subscription count: 1
  ```
* To check the meaning of the topic Type

  ```
  ros2 interface show geometry_msgs/msg/Twist
  ```
* Subscribe to a topic

  ```
  ros2 topic echo <topic_name>
  ```

### Service

* To check service info

  ```
  sudo apt install ros-humble-demo-nodes-cpp
  ros2 run demo_nodes_cpp add_two_ints_server
  ```
* In another terminal, check service and its type

  ```
  ros2 service list
  # /add_two_ints
  ros2 service type /add_two_ints
  # example_interfaces/srv/AddTwoInts
  ros2 interface show example_interfaces/srv/AddTwoInts
  # int64 a
  # int64 b
  # ---
  # int64 sum
  ```
* Call ther service in terminal

  ```
  ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{'a':1,'b':2}"
  #waiting for service to become available...
  #requester: making request: example_interfaces.srv.AddTwoInts_Request(a=1, b=2)

  #response:
  #example_interfaces.srv.AddTwoInts_Response(sum=3)
  ```

## Reference

* [ROS2 tutorial by Robotics Back-End](https://youtu.be/Gg25GfA456o)
