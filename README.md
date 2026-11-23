# RoboFusion: Gesture-Based Manipulator Teleoperation with IMU + Vision Sensor Fusion

Teleoperate a 4-DOF OpenManipulator-X arm with your hand. A webcam tracks the hand, a trained classifier recognizes gestures and joint angles, and an MPU6050 IMU on the wrist is fused with the vision estimates to cut noise. Commands go to the arm through ROS 2, with Gazebo for simulation.

In our evaluation, fusing the IMU with vision raised gesture-recognition accuracy from **80% to 95%** and improved positioning accuracy by **50%** over the prior method.

Team project (WPI): Ankit Gole, Harsh Chhajed, Shivam Shinde, Shreya Boyane, Simran Chauhan. Forked from [Sonica-B/RoboFusion](https://github.com/Sonica-B/RoboFusion).

## How it works

1. **Hand tracking**: MediaPipe extracts 3D hand landmarks from the webcam feed.
2. **Gesture classification**: a TensorFlow/Keras model classifies nine gestures (neutral, grip open/close, move up/down, rotate left/right, forward/backward) and predicts joint angles.
3. **Sensor fusion**: MPU6050 IMU angles are fused with the vision predictions; an autoencoder-based denoiser (`Angle_Denoising.ipynb`) smooths the predicted angles.
4. **Arm control**: a ROS 2 controller (`RoboFusion/ros_gesture_controller.py`, `arm_controller.py`) maps gestures and angles to joint commands for the arm, tested in Gazebo and on hardware.

## Repository layout

| Path | Contents |
|---|---|
| `app.py` | Real-time loop: webcam, MediaPipe, classifier, ROS 2 publisher |
| `DataCollection/` | Gesture capture scripts, raw and processed datasets, labels |
| `MLModel/` | Model definition, training, trained model and scaler |
| `RoboFusion/` | ROS 2 arm controller code |
| `Angle_Denoising.ipynb` | Denoising of the predicted joint angles |
| `docs/PROJECT_PROPOSAL.md` | Original project proposal and team workflow |

## Run it

```bash
pip install -r requirements.txt
python app.py
```

A webcam is required. For arm control, source a ROS 2 environment with the OpenManipulator-X controllers or Gazebo model running first.

## Stack

Python, ROS 2, Gazebo, MediaPipe, TensorFlow/Keras, OpenCV, scikit-learn, MPU6050 IMU, OpenManipulator-X.
