# Autonomous Target Navigation with Object Delivery to a Human

**Authors**: Maryam Harakat (359826), Mohamed Taha Guelzim (355739), Muhammad Mikail Rais (402800)  
**Course**: COM-304 Final Project Report

---

## Abstract

This project implements a real-time ball detection and tracking system for a mobile robot using the OAK-D Lite stereo camera and the ROS 2 Humble middleware. The system leverages on-device YOLO inference provided by DepthAI, stereo depth data, and odometry feedback to estimate the relative position of a ball in 2D space. The main output of the system is a boolean flag indicating detection, and a position vector consisting of the estimated angle and distance to the ball. This data can be used for downstream navigation or manipulation tasks.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Related Work](#related-work)
3. [Method](#method)
4. [Experiment](#experiment)
5. [Conclusion and Limitations](#conclusion-and-limitations)
6. [Individual Contributions](#individual-contributions)

---

## Introduction

### A. What is the problem we want to solve?

This project aims to develop an autonomous robot capable of navigating through obstacle-filled environments to locate, push, and deliver a predefined object to a moving human recipient. The key challenge is designing a solution that doesn’t require specialized hardware, such as robotic arms or custom pushing mechanisms.

**Research questions:**

- How does a robot perceive its environment?
- How can a robot effectively navigate and manipulate objects without specialized hardware?
- How can it track and follow a moving human recipient in real-time?

### B. Why is it important?

Solving this problem advances autonomous robots in human-centric applications like elder assistance, package delivery, and warehouse logistics. By avoiding hardware modifications, our approach allows easy and scalable deployment.

**Broader question:**

- How can we make autonomous robots versatile without hardware changes?

### C. Objectives

- Detect a ball using YOLO on the OAK-D Lite VPU.
- Estimate the ball’s 2D relative position using stereo depth.
- Integrate odometry to compensate for delay.
- Publish clean outputs for robot navigation logic.

---

## Related Work

### A. Luxonis DepthAI Framework

We used DepthAI’s `YoloSpatialDetectionNetwork` to perform object detection and stereo-based depth estimation on-device using the Myriad X VPU.

### B. Visual Servoing and Object Following with ROS

Previous approaches rely on CPU-based processing and traditional vision methods like optical flow. We improve robustness and speed by using YOLO and depth sensing.

### C. VSLAM and Odometry Integration

We referenced ORB-SLAM (Mur-Artal et al., 2015), which combines visual data and odometry for spatial understanding.

---

## Method

### A. Space Exploration and Mapping

We integrated SLAM with custom frontier detection. A 2D occupancy grid was generated with LiDAR via ROS2 and RViz. Frontier points were selected based on proximity and size, then explored using Nav2.

To debug, we built a secondary map visualizing waypoints and accessible areas.

### B. Hardware-Native Object Detection

We ran YOLOv7 inference directly on the Oak-D Lite. Models were compiled into `.blob` format using Luxonis tools. We used the DepthAI library on the Turtlebot’s Raspberry Pi to execute detection in real time while SLAM was active.

### C. Navigation

#### 1. Locating the Ball

The robot captured images at intervals, using YOLO to identify sport balls (COCO index 32). We also included apple (48), orange (49), and frisbee (29) to increase tolerance. A 360° scan was performed at frontier points.

#### 2. Acquiring the Ball

Using stereo vision and disparity, we converted depth + angle data to (x, y) coordinates, corrected with odometry. Then, we navigated toward the estimated position using Nav2.

---

## Experiment

### A. Modular Testing Approach

We tested each module independently—exploration, detection, navigation—before full integration. This allowed isolated debugging and hypothesis validation.

### B. Space Exploration

We deployed the robot in:
- Open areas (e.g. CO building basement)
- Small rooms
- Amphitheaters

Initial failures in large spaces led to fixes:
- Dynamic waypoint list maintenance
- Map scaling for larger environments

### C. Ball Recognition

Initial tests with a stationary camera passed, but robot motion introduced blur. We resolved this by pausing for 1 second before each frame during 360° scans.

---

## Conclusion and Limitations

> *(Add your actual conclusions and limitations here if not already provided.)*

---

## Individual Contributions

> *(Add the individual breakdown of contributions from Maryam, Mohamed Taha, and Muhammad here.)*

---

## Notes

- YOLOv7 was used for optimized detection speed and accuracy.
- Nav2 and SLAM integration made it possible to run fully autonomous navigation without external control.
- DepthAI enabled compact, real-time inference on-device.

---

## Acknowledgments

We thank our instructors, mentors, and the COM-304 course for guidance and feedback.

---
