# Unitree Go2 Teleop & Vision Bridge (ROS 2 Jazzy)

## 🎯 Aim
To establish a stable, low-latency control link between a Laptop (ROS 2 Jazzy) and the Unitree Go2 robot, bridging standard robotics messages with proprietary hardware APIs.

## 🛠️ Technical Stack
* **Environment:** Dockerized ROS 2 Jazzy (Ubuntu 24.04)
* **Middleware:** CycloneDDS (Configured for Multicast)
* **Translation:** Python-based Logic Bridge (Twist ➔ JSON ➔ API ID 1008)
* **Vision:** GStreamer H.264 UDP Pipeline

## ✅ Key Achievements
* **Protocol Translation:** Successfully mapped standard `geometry_msgs/Twist` velocities to the robot's SDK2 motion commands using a custom Python bridge.
* **Network Optimization:** Resolved `ddsi_udp_conn_write` errors by manually configuring Linux multicast routing tables.
* **Low Latency:** Achieved sub-50ms control response and a real-time pilot-view video feed by bypassing heavy ROS image transport layers.

## 📺 Demo
[Drag and drop your screen recording video file here to upload it]

## ⚙️ Usage Instructions

### 1. Host Laptop Network Setup
Run these commands on your host laptop to ensure the multicast traffic can reach the robot's interface:
```bash
sudo ip route add 224.0.0.0/4 dev enp2s0
sudo ip route add 230.1.1.1 dev enp2s0
docker run -it --net=host --privileged -v ~/unitree_ros2_ws:/unitree_ros2_ws ros:jazzy

# Inside Docker Container:
source /opt/ros/jazzy/setup.bash
source /unitree_ros2_ws/src/unitree_ros2/install/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

Terminal A (The Bridge):

Bash
python3 bridge.py

Terminal B (The Keyboard Controller):

Bash
# Open a second terminal and exec into the container
docker exec -it $(docker ps -lq) bash
source /opt/ros/jazzy/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
ros2 run teleop_twist_keyboard teleop_twist_keyboard

Launch Live Video (Host Laptop)
Run this in a regular terminal on your laptop to view the Go2 camera feed:

gst-launch-1.0 udpsrc address=230.1.1.1 port=1720 ! application/x-rtp, media=video, encoding-name=H264 ! rtph264depay ! h264parse ! avdec_h264 ! videoconvert ! autovideosink sync=false

Future Scope
Middleware Migration: Transitioning from CycloneDDS to FastDDS to optimize bandwidth for 3D LiDAR PointCloud visualization in RViz.

Autonomy: Integrating YOLOv8 for autonomous object tracking using the established GStreamer feed.

License
This project is licensed under the MIT License - see the LICENSE file for details.


---

### **Final Pro-Step:**
After you paste this and **drag your video file** into the demo section, your GitHub will be a complete portfolio piece. It shows you know:
1.  **Docker** (DevOps for Robotics)
2.  **Networking** (Routes/Multicast)
3.  **Middleware** (DDS/RMW)
4.  **Hardware API** (SDK Integration)

**Would you like me to help you draft the final "Project Completion" message



