# EmbeddedLinux
Embedded Linux and Edge AI Based Vision System for Real- Time Facial Recognition and Engagement Analysis

Implementation of Proposed Methodology 
Title : Embedded Linux and Edge AI Based Vision System for Real-
Time Facial Recognition and Engagement Analysis

1. Introduction
Modern classrooms lack objective tools to quantify student's attentiveness in real
time. Manual observation is subjective, inconsistent, and impractical for large
groups. This project proposes a real-time, fully offline, edge-based vision system
deployed on the STM32MP157-DK2 platform running Embedded Linux.
The system detects multiple student faces in a classroom scene, classifies their
behavioral state into predefined categories, and computes an overall “Mental
presence” percentage for the class.
The solution is implemented entirely at the edge without cloud dependency, ensuring
low latency and data privacy.

2. Problem Statement
Traditional attendance and surveillance systems only confirm physical presence and
do not measure engagement or attentiveness. Current engagement assessment
methods are manual, subjective, and non-quantitative. Cloud-based AI monitoring
systems introduce privacy risks, latency, and infrastructure dependency. There is a
need for a real-time, offline, embedded solution capable of detecting individuals,
identifying them, and estimating engagement levels locally. The challenge is to
implement such an intelligent vision system within the computational constraints of
an embedded Linux platform.

3. System Architecture

Camera Sensor
   ↓
Linux Camera Driver (V4L2)
   ↓
Embedded Linux (Buildroot/Yocto)
   ↓
AI Inference Engine (Edge AI)
   ↓
Facial Recognition + Gesture Analysis
   ↓
Mental Presence Score Computation
   ↓
Application Layer (UI / Dashboard / Logs)



4.Implementation Phases
PHASE 1 — Hardware Setup (STM32MP157-DK2 + Camera)
The implementation begins with preparing the hardware platform using the
STM32MP157-DK2 development board. The board is powered using a 5V adapter
and connected to an HDMI display and USB keyboard for interaction.
A USB UVC-compatible webcam is connected directly to the board’s USB port. The
use of a UVC camera avoids the need for custom driver development since Linux
provides native support.
After powering the board, the boot sequence is verified through HDMI output. This
phase ensures that all hardware components are properly connected and operational
before software configuration begins.Outcome:
• Board boots successfully
• Peripheral devices detected
• Camera physically connected

PHASE 2 — Install OpenSTLinux (Yocto Based)
The embedded Linux environment is installed using the official OpenSTLinux SD
card image provided by STMicroelectronics.
The image is flashed onto a microSD card and inserted into the STM32MP157-DK2
board. After booting, the system is accessed through the terminal interface.
Basic verification steps include:
• Checking kernel version
• Verifying USB subsystem
• Confirming system stability
This phase establishes a stable embedded Linux platform required for camera
interfacing and AI deployment.
Outcome:
• Linux boot successful
• System login verified
• USB subsystem operational

PHASE 3 — Verify Camera Pipeline
The camera interface is validated using the Linux Video4Linux (V4L2) framework.
The presence of /dev/video0 confirms successful driver loading. Camera capability
tools are used to inspect supported formats and resolutions.A live streaming test is performed to confirm real-time frame acquisition. This
ensures the complete camera pipeline—from hardware to display—is functioning
correctly.
Outcome:
• Camera detected by OS
• Video streaming verified
• Stable frame acquisition confirmed

PHASE 4 — AI Model Training (On PC)
Model development is performed on a separate Ubuntu system to avoid overloading
the embedded board.
A structured dataset is created with four behavioral classe...
1)Attentive 
2)Attentive with no understanding 
3)Disattentive 
Images are manually labeled using folder-based classification. A lightweight
Convolutional Neural Network (CNN) is trained using TensorFlow. Images are
resized to a fixed resolution and normalized before training.
The dataset is split into training and validation sets to evaluate performance. After
convergence, the model is saved in Keras format.
Output Artifact:
emotion_model.h5

PHASE 5 — Convert Model for Edge Deployment
To enable efficient execution on STM32MP157, the trained model is converted to
TensorFlow Lite format.Post-training optimization is applied to reduce model size and improve inference
speed. The converted model is tested locally using the TFLite interpreter to validate
prediction consistency.
Deployment Model:
emotion_model.tflite
This step ensures compatibility with limited embedded hardware resources.

PHASE 6 — Deploy to STM32MP157 DK2
The optimized TFLite model is transferred to the STM32 board. Required runtime
libraries such as OpenCV and TFLite runtime are installed.
A Python-based application is implemented to:
1. Capture frames from the webcam
2. Perform face detection using OpenCV
3. Crop and resize face regions
4. Run emotion classification using TFLite
5. Overlay predictions on the live display
The system is tested under real-time conditions to ensure stability and acceptable
performance.
Outcome:
• Real-time emotion prediction operational
• Multi-face detection functioning
• System running fully offline

PHASE 7— Final Application Logic
The final logic layer computes classroom mental presence.
For each frame:
1. Detect total number of faces
2. Sum behavioral scores
3. Compute average
4. Convert to percentage
The calculated percentage represents overall classroom attentiveness and is
displayed in real time.
The system operates independently of cloud services and processes all data locally
on the embedded platform.

5.Conclusion
The above phases describe the structured methodology proposed for developing an
embedded edge-based classroom mental presence monitoring system. The approach
outlines the complete workflow starting from hardware setup and embedded Linux
configuration to AI model training, optimization, and deployment on the
STM32MP157 platform.
This document presents the planned implementation strategy rather than final results.
Each phase has been logically organized to ensure systematic development,
validation, and integration of hardware and software components. The methodology
emphasizes modular design, offline processing, and efficient use of embedded
resources.
The next stage of the project will focus on executing the defined phases, validating
system performance under real classroom conditions, and optimizing real-time
behavior detection accuracy.
Thus, this document serves as the implementation roadmap for the proposed
embedded AI-based classroom mental presence estimation system.
