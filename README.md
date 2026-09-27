# Intelligent Conveyor Belt Health Monitoring and Predictive Maintenance System

## Overview

An intelligent conveyor belt health monitoring and predictive maintenance system designed for detecting conveyor belt damage and monitoring abnormal operating conditions.

The system combines computer vision with sensor-based monitoring to provide a multimodal view of conveyor belt health.

A Logitech C370 webcam captures the conveyor belt, while YOLOv11 is used for visual damage detection. An ESP32 collects real-time data from multiple sensors including current, rotational speed, vibration, distance, and thermal information.

The collected information can be analyzed together to identify abnormal conveyor conditions and generate an overall health status.

---

## Problem Statement

Conveyor belts used in mining and material transportation are exposed to continuous mechanical stress, excessive loading, misalignment, wear, overheating, and other operating conditions.

Conventional inspection methods may depend on manual inspection or periodic maintenance, which can result in delayed detection of developing faults.

The proposed system aims to continuously monitor conveyor belt conditions using:

- Computer vision
- Current monitoring
- Rotational speed monitoring
- Vibration monitoring
- Distance monitoring
- Thermal monitoring
- Machine learning

The objective is to detect visible belt damage and identify abnormal operating conditions at an early stage.

---

## Objectives

1. Detect visible conveyor belt damage using computer vision.
2. Monitor conveyor operating parameters using sensors.
3. Identify abnormal vibration and temperature conditions.
4. Monitor motor/conveyor current variations.
5. Monitor rotational speed using a Hall-effect sensor.
6. Monitor belt distance/profile using an ultrasonic sensor.
7. Detect thermal abnormalities using an MLX90640 thermal sensor.
8. Combine visual and sensor-based information for conveyor health assessment.
9. Store monitoring data for historical analysis.
10. Provide a basis for predictive maintenance and early fault detection.

---

## System Architecture

```text
                    ┌──────────────────────┐
                    │     CONVEYOR BELT    │
                    └──────────┬───────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼
      ┌─────────────────┐           ┌─────────────────┐
      │  Logitech C270  │           │     Sensors     │
      │  Vision Input   │           │ Vibration/Load  │
      └────────┬────────┘           │ Temp/Speed etc. │
               │                    └────────┬────────┘
               ▼                             │
      ┌─────────────────┐                    ▼
      │     OpenCV      │           ┌─────────────────┐
      │ Frame Capture & │           │      ESP32      │
      │ Pre-processing  │           │ Sensor Gateway  │
      └────────┬────────┘           └────────┬────────┘
               │                             │
               ▼                             ▼
      ┌─────────────────┐           ┌─────────────────┐
      │    YOLO Model   │           │ Feature         │
      │ Damage Detection│           │ Extraction      │
      └────────┬────────┘           └────────┬────────┘
               │                             │
               ▼                             ▼
      ┌─────────────────┐           ┌─────────────────┐
      │ Visual Damage   │           │    XGBoost      │
      │ Classification  │           │ Risk Prediction │
      └────────┬────────┘           └────────┬────────┘
               │                             │
               └──────────────┬──────────────┘
                              ▼
                    ┌────────────────────┐
                    │   Fusion / Backend │
                    │    Data Layer      │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │     Dashboard      │
                    │                    │
                    │ • Live Camera      │
                    │ • Damage Detection │
                    │ • Sensor Data      │
                    │ • Risk Prediction  │
                    │ • Alerts           │
                    │ • History          │
                    └────────────────────┘
                            |
                    Monitoring Dashboard
```

