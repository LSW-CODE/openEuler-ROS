在 ROS 2 中切换底层 DDS 集成的通信插件并与 MQTT 进行通信：
1. 环境准备 更新系统和安装依赖

2. 安装 DDS 实现

3. 安装 MQTT 包


4. 配置 DDS


# 文件名: dds_config.yaml
ros__parameters:
  use_rmw: "rmw_fastrtps_cpp"  # 使用 Fast DDS

5. 编写 MQTT 节点

创建一个 ROS 2 节点，使用 MQTT API 进行消息的发布和订阅。

import rclpy
from rclpy.node import Node
from mqtt_client import MQTTClient

class MqttNode(Node):
    def __init__(self):
        super().__init__('mqtt_node')
        self.mqtt_client = MQTTClient('broker_address', 1883)
        self.mqtt_client.connect()
        self.mqtt_client.subscribe('topic')
        self.mqtt_client.on_message = self.on_message

    def on_message(self, msg):
        self.get_logger().info(f'Received MQTT message: {msg}')

    def publish_message(self, message):
        self.mqtt_client.publish('topic', message)

def main(args=None):
    rclpy.init(args=args)
    node = MqttNode()
    rclpy.spin(node)
    rclpy.shutdown()

6. 运行节点



ros2 run your_package your_node --ros-args --params-file path/to/dds_config.yaml


7. 测试与验证

发布和订阅消息，确保 MQTT 主题上的消息能够被成功接收和发送。
