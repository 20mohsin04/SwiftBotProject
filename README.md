# SwiftBot: Autonomous Light-Tracking Rover 🤖

> An embedded autonomous rover engineered to detect, track, and navigate towards light sources in real-time using custom computer vision algorithms.

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-Raspberry_Pi-C51A4A?style=for-the-badge&logo=Raspberry-Pi&logoColor=white)
![Embedded Systems](https://img.shields.io/badge/System-Embedded-blue?style=for-the-badge)

## 💡 Overview
SwiftBot is an autonomous vehicle project built on the **Raspberry Pi 4**. Unlike standard line-followers that use simple IR sensors, SwiftBot utilizes a **camera-based feedback loop**. It captures raw image data, processes pixel intensity in real-time using a custom-written Java algorithm, and adjusts motor speed differentially to "chase" the brightest point in its field of view.

## 🛠️ Tech Stack & Hardware
* **Language:** Java (JDK 11+)
* **Platform:** Raspberry Pi OS (Headless)
* **Libraries:** Pi4J (GPIO Control), Native Image IO
* **Hardware:**
    * Raspberry Pi 4 Model B
    * Pi Camera Module V2
    * L298N Motor Driver Controller
    * DC Gear Motors x2 + Chassis

## ⚙️ Key Engineering Features

### 1. Custom Computer Vision Algorithm
Instead of relying on heavy libraries like OpenCV for simple tracking, I implemented a lightweight pixel-analysis algorithm to minimize latency:
* **Input:** Captures 640x480 frames from the Pi Camera.
* **Processing:** Iterates through pixel arrays to calculate the "Center of Brightness" (weighted average of luminance).
* **Output:** Determines a vector (Left/Right/Center) in **<200ms**.

### 2. Multi-Threaded Architecture
To ensure smooth motor movement while processing heavy image data, the system uses a threaded architecture:
* **Vision Thread:** Continuously polls the camera and calculates the target vector.
* **Control Thread:** Reads the vector and adjusts PWM signals to the L298N driver instantly.
* **Result:** Prevents "stuttering" during image processing spikes.

## 🚀 How to Run
1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/20mohsin04/SwiftBot.git](https://github.com/20mohsin04/SwiftBot.git)
    cd SwiftBot
    ```
2.  **Build the project:**
    ```bash
    javac -cp .:classes:/opt/pi4j/lib/'*' src/*.java -d classes
    ```
3.  **Run (Requires Root for GPIO access):**
    ```bash
    sudo java -cp .:classes:/opt/pi4j/lib/'*' Main
    ```

## 🔮 Future Improvements
* Implement PID control for smoother turning arcs.
* Add ultrasonic sensors for obstacle avoidance priority.
* Migrate image processing to Python via socket communication for advanced object detection.
