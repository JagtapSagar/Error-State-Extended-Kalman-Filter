Vehicle State Estimation on a Roadway
===

[Error-State Extended Kalman Filter](#Error-State-Extended-Kalman-Filter)//[How to run the code](#How-to-run-the-code)//[Results](#Results)

The aim of this exercise is to estimate vehicle state based on Inertial Measurement Unit (IMU), LIDAR point cloud data and GPS Global Navigation Satellite System (GNSS) data.
The IMU data is provided for each time step and consists of Specific force data and rotational velocity data in the vehicle frame. The set of LIDAR point cloud data and GPS data may not be available at every time step. 

The project has three parts:
1. Implement Error-State Extended Kalman Filter (ES-EKF) using IMU data for prediction step and LIDAR point cloud and GPS for correction when available.
2. Introduce errors in LIDAR sensor calibration to observe its effects and adjust filter parameters to account for it.
3. Explore the effects of sensor dropout, that is, when all external positioning information (from GPS and LIDAR) is lost for a short period of time.

Error-State Extended Kalman Filter
---
An ES-EKF was implemented to estimate the vehicle state as accurately as possible. ES-EKF mainly consists of three steps: namely prediction, measurement, and correction. The prediction step employs the vehicle's dynamic model to predict the current state of the vehicle using IMU data. The IMUs generally do not have very high precision and therefore the predictions can deviate largely overtime if only IMU data is used for vehicle state estimation. Therefore, LIDAR point cloud and GNSS data is measured in the measurement step whenever available to get a better idea of the ground truth values since the error rates for these measurements are very low. In real world scenarios, these measurement sources are not available for calculation at each timestep since it may depend on the LIDAR sensor scan rate, GPS connectivity and hardware refresh rates. When measurement data is not available a rough estimate using the prediction stage might suffice; However, when measurement data is available, the correction step is used to penalize the systems' state estimation errors and improve estimation accuracy.

The implementation of ES-EKF algorithm in the update loop is as follows.
1. Update nominal state with motion model
2. Propagate uncertainty
3. If a measurement is available:</br>
  a. Compute Kalman Gain</br>
  b. Compute error state</br>
  c. Correct nominal state</br>
  d. Correct state covariance

The illustration below shows the above pseudocode in a workflow diagram.

<img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/ES_EKF.jpg'>
 

NOTE:
* The ES-EKF is implemented in es_ekf.py and uses rotations.py for Quaternion calculations.
* The equations used in this project can be found in the ES-EKF lecture slides in this repository.
* For information on Quaternion transformations refer the Quaternion kinematics for the error-state KF pdf in this repository.
* The CARLA Simulator data used for calculations is contained in .pkl file in the data folder.
* This project was a part of course evaluation for the [State Estimation and Localization for Self-Driving Cars course by University of Toronto](https://www.coursera.org/learn/state-estimation-localization-self-driving-cars?specialization=self-driving-cars).
* The ground truth data was only provided till about 70-80% of the actual vehicle path as rest of it was kept hidden for grading purposes.


How to run the code
---
To run this project:

1. Download the contents of this directory.
2. Execute es_ekf.py
    `python es_ekf.py`

NOTE: python version 3.6 was used with matplotlib version 3.0.0

Results
---
Part 1 Results : ES-EKF using Inertia Measurement Unit (IMU) data for prediction step and LIDAR point cloud and GPS for correction.
Vehicle state estimates | State estimation error plot
:-------------------------:|:-------------------------:
<img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_1.png'> | <img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_1_error_plots.png'>

Part 2 Results : With adjusted filter parameters to account for calibration errors introduced in LIDAR sensor.
Vehicle state estimates | State estimation error plot
:-------------------------:|:-------------------------:
<img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_2.png'> | <img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_2_error_plots.png'>

Part 3 Results : Effects of sensor dropout, that is, when all external positioning information (from GPS and LIDAR) is lost for a short period of time.
Vehicle state estimates | State estimation error plot
:-------------------------:|:-------------------------:
<img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_3.png'> | <img src='https://github.com/JagtapSagar/Error-State-Extended-Kalman-Filter/blob/main/Images/Part_3_error_plots.png'>




