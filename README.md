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

## Sensors

## ESP32

    The ESP32 acts as the main sensor acquisition and communication controller.
    
    It collects data from the connected sensors and sends the readings to the processing system.
    
    Responsibilities include:
    
    Sensor interfacing
    Data acquisition
    Timestamping sensor readings
    Basic preprocessing
    Communication with the laptop
    
## ACS712 Current Sensor

    The ACS712 is used to monitor electrical current.
    
    Current variations can provide information about changes in motor/conveyor operating conditions.
    
    For example, an increase in current under similar operating conditions may indicate increased mechanical load or resistance.
    
    The sensor output is collected by the ESP32.

## A3144 Hall Sensor

    The A3144 Hall-effect sensor is used to detect rotational movement.
    
    A rotating magnetic element can generate pulses which can be counted by the ESP32.
    
    The pulse frequency can be used to estimate rotational speed.
    
    Rotating Element
           |
         Magnet
           |
           v
       A3144 Sensor
           |
          Pulse
           |
          ESP32
           |
       Pulse Counting
           |
       Speed Estimation
    
    The speed information can be used along with current and vibration readings to identify abnormal operating conditions.

## ADXL345 Accelerometer

    The ADXL345 is a 3-axis accelerometer used for vibration monitoring.
    
    It measures acceleration along:
    
    X-axis
    Y-axis
    Z-axis
    
    Vibration features can be extracted from the acceleration data to identify abnormal mechanical behavior.
    
    Potential features include:
    
    Mean acceleration
    RMS acceleration
    Peak acceleration
    Standard deviation
    Frequency-domain features
    
    These features can later be used by the machine learning pipeline.

## HC-SR04 Ultrasonic Sensor

    The HC-SR04 is used for non-contact distance measurement.
    
    It can be positioned relative to the conveyor belt to monitor changes in distance.
    
    Changes in measured distance can provide information related to:
    
    Belt position
    Belt profile
    Sag or displacement
    Material buildup
    Abnormal positional changes
    
    The ultrasonic measurements are collected by the ESP32.

## MLX90640 Thermal Sensor

    The MLX90640 is a thermal imaging sensor that provides a temperature distribution across its field of view.
    
    It can be used to identify abnormal thermal patterns around conveyor components.
    
    Potential thermal features include:
    
    Maximum temperature
    Average temperature
    Minimum temperature
    Temperature variance
    Hotspot location
    Hotspot area
    
    Thermal information can be combined with other sensor measurements for condition monitoring.

