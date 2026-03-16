# Unitree Go2 Teleop & Vision Bridge (ROS 2 Jazzy)

## 🎯 Aim
To establish a stable, low-latency control link between a Laptop (ROS 2 Jazzy) and the Unitree Go2 robot, bridging standard robotics messages with proprietary hardware APIs.

## 🛠️ Technical Stack
* **Environment:** Dockerized ROS 2 Jazzy
* **Middleware:** CycloneDDS (Configured for Multicast)
* **Translation:** Python-based Logic Bridge (Twist ➔ JSON ➔ API ID 1008)
* **Vision:** GStreamer H.264 UDP Pipeline

## ✅ Key Achievements
* **Protocol Translation:** Successfully mapped $X, Y, Z$ velocities to the robot's SDK2 motion commands.
* **Network Optimization:** Resolved `ddsi_udp_conn_write` errors by manually configuring Linux multicast routing tables.
* **Low Latency:** Achieved sub-50ms control response and real-time pilot-view video feed.

## 📺 Demo
[Drag your Screen Recording video file here to upload it]

## 🚀 Future Scope
* Migrating from **CycloneDDS to FastDDS** to optimize bandwidth for 3D LiDAR PointCloud visualization in RViz.
* Integrating YOLOv8 for autonomous object tracking.
