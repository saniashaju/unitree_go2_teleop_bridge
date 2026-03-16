# Unitree Go2 Teleop & Vision Bridge (ROS 2 Jazzy)

## 🎯 Aim
To establish a stable, low-latency control link between a Laptop (ROS 2 Jazzy) and the Unitree Go2 robot, bridging standard robotics messages with proprietary hardware APIs.

## 🛠️ Technical Stack
* **Environment:** Dockerized ROS 2 Jazzy (Ubuntu 24.04)
* **Middleware:** CycloneDDS (Configured for Multicast)
* **Translation:** Python-based Logic Bridge (Twist ➔ JSON ➔ API ID 1008)
* **Vision:** GStreamer H.264 UDP Pipeline

## ✅ Key Achievements
* **Protocol Translation:** Successfully mapped standard `geometry_msgs/Twist` velocities to the robot's SDK2 motion commands.
* **Network Optimization:** Resolved `ddsi_udp_conn_write` errors by manually configuring Linux multicast routing tables.
* **Low Latency:** Achieved sub-50ms control response and a real-time pilot-view video feed by bypassing heavy ROS image transport layers.

## 📺 Demo
## 📺 Demo
> **Hardware Verification:** Real-time teleoperation and vision stack demonstration.

<p align="center">
  <video src="Teleop.mp4" width="100%" controls>
  </video>
</p>

### Technical Highlights:
* **Motion:** Sub-50ms latency using Python-to-JSON API translation.
* **Vision:** Direct GStreamer UDP pipeline for real-time H.264 streaming
  
---

## ⚙️ Usage Instructions

### Step 1: Host Laptop Network Setup
Run these commands in a regular terminal on your laptop to fix the multicast routes:
```bash
sudo ip route add 224.0.0.0/4 dev enp2s0
sudo ip route add 230.1.1.1 dev enp2s0
Step 2: Initialize Docker Environment
Launch the container and source the workspace:

Bash
docker run -it --net=host --privileged -v ~/unitree_ros2_ws:/unitree_ros2_ws ros:jazzy

# Inside Docker:
source /opt/ros/jazzy/setup.bash
source /unitree_ros2_ws/src/unitree_ros2/install/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
Step 3: Start the Control Bridge (Terminal A)
Run the translation logic:

Bash
python3 bridge.py
Step 4: Launch Keyboard Controller (Terminal B)
In a new terminal, enter the container and start the teleop node:

Bash
docker exec -it $(docker ps -lq) bash
source /opt/ros/jazzy/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
ros2 run teleop_twist_keyboard teleop_twist_keyboard
Step 5: Physical Hardware Handover
Follow this sequence on the Unitree Remote to allow laptop control:

Press L2 + Start (Listen for the beep).

Press L2 + B (Switches to Sport Mode for external commands).

Step 6: Launch Live Vision Feed
Run this on your host laptop (not in Docker) to see the camera:

Bash
gst-launch-1.0 udpsrc address=230.1.1.1 port=1720 ! application/x-rtp, media=video, encoding-name=H264 ! rtph264depay ! h264parse ! avdec_h264 ! videoconvert ! autovideosink sync=false
🚀 Future Scope
Middleware Migration: Transitioning from CycloneDDS to FastDDS to optimize bandwidth for 3D LiDAR PointCloud visualization in RViz.

Autonomy: Integrating YOLOv8 for autonomous object tracking using the established GStreamer feed.

📜 License
This project is licensed under the MIT License - see the LICENSE file for details.


---

### **How to finalize your GitHub now:**
1. **Pencil Icon:** Go to GitHub and click the edit icon on your README.
2. **Select All & Delete:** Clear everything out.
3. **Paste:** Paste the text I gave you above.
4. **Drag Video:** Drag your video file into the `[Drag and drop...]` section.
5. **Commit:** Hit the green button to save.

**Would you like me to draft a message you can send to your mentor with the link to this repo?**

