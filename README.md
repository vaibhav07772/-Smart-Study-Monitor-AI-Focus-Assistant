# 🧠 Smart Study Monitor — AI Focus Assistant

<p align="center">
  <b>Real-Time Computer Vision System for Drowsiness, Phone Distraction & Focus Monitoring</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-blue?logo=python" alt="Python"/>
  <img src="https://img.shields.io/badge/OpenCV-Computer%20Vision-green?logo=opencv" alt="OpenCV"/>
  <img src="https://img.shields.io/badge/MediaPipe-Face%20Landmarks-orange" alt="MediaPipe"/>
  <img src="https://img.shields.io/badge/YOLOv8-Object%20Detection-purple" alt="YOLOv8"/>
  <img src="https://img.shields.io/badge/Pygame-Audio%20Alerts-red" alt="Pygame"/>
</p>

---

## 📌 Overview

**Smart Study Monitor** is a real-time AI-powered computer vision application designed to help students maintain focus during study sessions.

The system uses a webcam to monitor the user's visual behavior and detects three major distractions:

* 😴 **Drowsiness** — detects prolonged eye closure
* 📱 **Phone distraction** — detects a mobile phone using YOLOv8
* 👀 **Loss of focus** — monitors whether the user is looking away

When a distraction is detected, the system provides an **audio alert** to bring the user's attention back to studying.

> This project is an educational computer-vision productivity assistant. It is not a medical or clinical drowsiness detection system.

---

# ✨ Key Features

| Feature                 | Description                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------ |
| 😴 Drowsiness Detection | Uses facial landmarks and eye-opening measurements to identify prolonged eye closure |
| 📱 Phone Detection      | Uses YOLOv8n to detect mobile phones in the camera frame                             |
| 👀 Focus Monitoring     | Monitors face orientation/attention and detects when the user looks away             |
| 🔊 Audio Alerts         | Plays different sounds for different distraction events                              |
| 🎥 Real-Time Processing | Processes webcam frames continuously                                                 |
| ⚡ Lightweight Detection | Combines landmark-based detection with YOLOv8 Nano                                   |
| 🖥️ Local Application   | Runs locally using the computer's webcam                                             |

---

# 🎯 Problem Statement

While studying for long periods, students can lose concentration because of:

* Sleepiness
* Mobile-phone usage
* Looking away from the study environment
* Reduced attention over time

The goal of this project is to create a simple **AI-based study companion** that observes these signals in real time and provides immediate feedback.

### Basic Idea

```text
              Webcam
                 │
                 ▼
        ┌─────────────────┐
        │  Video Capture  │
        └────────┬────────┘
                 │
                 ▼
        ┌─────────────────┐
        │ Frame Processing│
        └────────┬────────┘
                 │
        ┌────────┼───────────────┐
        │        │               │
        ▼        ▼               ▼
     Face      Phone          Focus
   Landmarks  Detection      Monitoring
        │        │               │
        ▼        ▼               ▼
   Drowsiness  YOLOv8       Looking Away
   Detection   Detection      Detection
        │        │               │
        └────────┼───────────────┘
                 ▼
          ┌──────────────┐
          │ Alert System │
          └──────┬───────┘
                 ▼
            🔊 Audio Alert
```

---

# 🧠 How It Works

## 1. Webcam Capture

The application continuously captures frames from the computer's webcam using OpenCV.

```text
Webcam
  ↓
OpenCV VideoCapture
  ↓
Frame
  ↓
AI Detection
```

Each frame is analyzed in real time.

---

## 2. 😴 Drowsiness Detection

The system uses facial/eye landmark information to estimate whether the user's eyes remain closed for a configured duration.

An **EAR-style eye-opening measurement** is used to represent the relationship between vertical and horizontal eye distances.

Conceptually:

```text
Eye Opening
     ↓
Eye Landmark Detection
     ↓
Eye Ratio / EAR-style Measurement
     ↓
Compare with Threshold
     ↓
Prolonged Eye Closure?
     ↓
Yes → 🔊 Drowsiness Alert
```

A threshold around **0.20** is used in the project logic as an indication of reduced eye opening.

If the eyes remain closed beyond the configured duration, the application triggers:

```text
alarm.mp3
```

### Example

```text
Normal Eyes
    ↓
Student continues studying

Eyes Closed
    ↓
Timer starts

Prolonged Closure
    ↓
😴 Drowsiness Detected

    ↓

🔊 Wake-up Alert
```

---

# 📱 3. Phone Distraction Detection

The project uses **YOLOv8n (YOLOv8 Nano)** for real-time object detection.

The model detects the **cell phone class** from the COCO object-detection classes.

```text
Webcam Frame
     ↓
YOLOv8n
     ↓
Object Detection
     ↓
Cell Phone Detected?
     ↓
Yes
     ↓
📱 Phone Alert
```

When a phone is detected above the configured confidence threshold, the application plays:

```text
paudio.mp3
```

The goal is not to identify what the user is doing on the phone, but simply to detect the presence of a mobile phone in the monitored scene.

---

# 👀 4. Focus Monitoring

The application also monitors the user's face orientation/attention using facial landmark information.

Simplified flow:

```text
Face Detected
     ↓
Facial Landmarks
     ↓
Estimate Face Direction
     ↓
Looking Away?
     ↓
Yes
     ↓
👀 Focus Alert
```

When the user repeatedly looks away from the study area, the system can trigger a focus-related audio alert.

---

# 🔊 5. Audio Alert System

Different events can trigger different sounds:

| Event                   | Audio        |
| ----------------------- | ------------ |
| 😴 Drowsiness           | `alarm.mp3`  |
| 📱 Phone detected       | `paudio.mp3` |
| 👀 Focus / looking away | `faudio.mp3` |

Audio playback is handled using **Pygame Mixer**.

The purpose of using different sounds is to make the feedback distinguishable for different types of distraction.

---

# 🔄 Complete Processing Pipeline

```text
                   START
                     │
                     ▼
              Open Webcam
                     │
                     ▼
             Capture Video Frame
                     │
                     ▼
          ┌──────────────────────┐
          │   Frame Processing   │
          └──────────┬───────────┘
                     │
        ┌────────────┼─────────────┐
        │            │             │
        ▼            ▼             ▼
   Face/Eye       YOLOv8n       Face Direction
   Landmarks      Detection      Monitoring
        │            │             │
        ▼            ▼             ▼
   Eye Status     Phone?       Looking Away?
        │            │             │
        ▼            ▼             ▼
   Drowsy?        Yes → Alert    Yes → Alert
        │
        ▼
  Prolonged Closure?
        │
       Yes
        │
        ▼
   🔊 Wake-up Alert
        │
        ▼
   Continue Monitoring
```

---

# 🛠️ Technology Stack

| Technology       | Purpose                             |
| ---------------- | ----------------------------------- |
| **Python**       | Core programming language           |
| **OpenCV**       | Webcam capture and image processing |
| **MediaPipe**    | Facial/landmark-based analysis      |
| **YOLOv8n**      | Mobile-phone/object detection       |
| **Pygame Mixer** | Audio alert playback                |
| **Conda**        | Environment management              |

---

# 📂 Project Structure

```text
Smart-Study-Monitor-AI-Focus-Assistant/
│
├── app.py
│
├── yolov8n.pt
├── face_landmarker.task
├── hand_landmarker.task
│
├── alarm.mp3
├── paudio.mp3
├── faudio.mp3
│
├── requirements.txt
│
└── README.md
```

### Important Files

| File                   | Purpose                 |
| ---------------------- | ----------------------- |
| `app.py`               | Main application        |
| `yolov8n.pt`           | YOLOv8 Nano model       |
| `face_landmarker.task` | Face landmark model     |
| `hand_landmarker.task` | Hand landmark model     |
| `alarm.mp3`            | Drowsiness alert        |
| `paudio.mp3`           | Phone distraction alert |
| `faudio.mp3`           | Focus alert             |
| `requirements.txt`     | Python dependencies     |

---

# 🚀 Installation

## 1. Clone the Repository

```bash
git clone https://github.com/vaibhav07772/-Smart-Study-Monitor-AI-Focus-Assistant.git
cd -Smart-Study-Monitor-AI-Focus-Assistant
```

---

## 2. Create Conda Environment

Python 3.10 is recommended for this project.

```bash
conda create -n study_monitor python=3.10 -y
```

Activate the environment:

```bash
conda activate study_monitor
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

If the project dependencies are already listed correctly in `requirements.txt`, this will install the required packages.

---

# ▶️ Run the Application

Start the application with:

```bash
python app.py
```

The application will open the webcam and begin monitoring.

To stop the application:

```text
Press Q
```

---

# 🧪 Detection Logic

| Component          | Logic                                                            |
| ------------------ | ---------------------------------------------------------------- |
| 👁️ Eye Detection  | Facial landmark-based eye analysis                               |
| 😴 Drowsiness      | Low eye-opening measurement maintained for a configured duration |
| 📱 Phone           | YOLOv8n object detection                                         |
| 🎯 Phone Threshold | Confidence threshold configured by the application               |
| 👀 Focus           | Face direction/attention monitoring                              |
| 🔊 Alerts          | Pygame audio playback                                            |

---

# 📊 Example Session

A typical session can behave like this:

```text
Student starts studying
        │
        ▼
Camera detects face
        │
        ▼
Eyes open → Normal
        │
        ▼
Student becomes sleepy
        │
        ▼
Eyes remain closed
        │
        ▼
😴 Drowsiness detected
        │
        ▼
🔊 Wake-up alarm
```

Another scenario:

```text
Student starts using phone
        │
        ▼
YOLOv8 detects mobile phone
        │
        ▼
📱 Phone detected
        │
        ▼
🔊 Phone distraction alert
```

---

# 🎓 What This Project Demonstrates

This project demonstrates practical implementation of:

* Computer Vision
* Real-time video processing
* Facial landmark analysis
* Object detection
* YOLO-based inference
* Attention/focus monitoring
* Audio event handling
* Threshold-based decision logic
* Python application development
* AI-powered productivity tools

---

# 💼 Interview / Viva Explanation

### "Tell me about your project."

> **Smart Study Monitor is a real-time computer vision application that acts as an AI-based study focus assistant. It uses OpenCV for webcam processing, MediaPipe-based facial landmarks for eye and face analysis, and YOLOv8 Nano for mobile-phone detection. The system monitors drowsiness, phone distraction, and loss of focus. When one of these conditions is detected, it provides an audio alert using Pygame. The project helped me understand how multiple computer-vision models and rule-based decision logic can be combined into a real-time AI application.**

### Why YOLOv8?

> YOLOv8 is suitable for real-time object detection because it can process video frames efficiently while providing object locations and confidence scores.

### Why MediaPipe?

> MediaPipe provides efficient landmark-based perception capabilities that can be used for facial and hand analysis without requiring a large custom object-detection model.

### What happens when the phone is detected?

> The webcam frame is passed to the YOLOv8 Nano detector. If a mobile phone is detected above the configured confidence threshold, the application triggers the phone-distraction audio alert.

### How is drowsiness detected?

> The system analyzes eye-related facial landmarks and calculates an eye-opening measurement. If the measurement remains below the configured threshold for a sufficient duration, the system considers the user potentially drowsy and triggers an alert.

---

# ⚠️ Limitations

The current system can be affected by:

* Poor lighting
* Camera quality
* Extreme head angles
* Face partially outside the camera frame
* Occlusion of the eyes
* Phone detection confidence
* Webcam performance
* Different individual facial characteristics

The drowsiness component is a **computer-vision heuristic for productivity assistance**, not a medical diagnosis or safety-critical drowsiness detection system.

---

# 🔐 Privacy

The application is designed to operate locally using the computer's webcam.

The project does not require uploading webcam footage to a remote server as part of its documented workflow.

Users should still review the implementation and dependencies before deploying the application in environments where privacy requirements are important.

---

# 🚀 Future Improvements

Possible extensions include:

* 📊 Daily focus-time analytics
* ⏱️ Study session tracking
* 📈 Productivity reports
* 🧠 Personalized focus patterns
* 📱 Telegram/WhatsApp notifications
* 🗓️ Daily study summaries
* 🏆 Focus streaks and productivity scores
* 🌐 Web dashboard
* 💾 Session history
* 🔔 Configurable notification system

---

# 🧠 Learning Outcomes

Through this project, I explored:

```text
Computer Vision
      ↓
Real-Time Video Processing
      ↓
Facial Landmark Analysis
      ↓
Object Detection
      ↓
Decision Logic
      ↓
Audio Feedback
      ↓
AI-Powered Productivity Application
```

The project provided hands-on experience in combining multiple AI components into a single real-time application.

---

# 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

```bash
git fork
git clone
git checkout -b feature/your-feature
```

Make your changes and submit a pull request.

---

# 📜 License

This project is intended for educational and research purposes.

If a specific license file is added to the repository, refer to that license for the applicable terms.

---

# 👨‍💻 Author

**Vaibhav Singh**

Aspiring AI/ML Engineer | Generative AI | Agentic AI | RAG | MLOps

* GitHub: [@vaibhav07772](https://github.com/vaibhav07772)
* LinkedIn: [Vaibhav Singh](https://www.linkedin.com/in/vaibhav07772/)

---

<p align="center">
  ⭐ If you find this project useful, consider giving it a star!
</p>

<p align="center">
  <b>Built with Python, Computer Vision & AI 🤖</b>
</p>
