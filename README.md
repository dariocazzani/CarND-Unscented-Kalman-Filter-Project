# Object Tracking using Lidar and Radar sensor data - Unscented Kalman Filter
Self-Driving Car Engineer Nanodegree Program

## Goal of this project
Uilize a Kalman filter to estimate the state of a moving object of interest with noisy lidar and radar measurements.
The goal is to obatin low RMSE values for the estimated parameters: position in the x and y axis, and velocity in the x and y axis.

## Data
This project involves the Term 2 Simulator from Udacity which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases)
Data is read from and to the Simulator by the C++ program. See file `main.cpp`

## Reading data from the Simulator:
This repository includes two files that can be used to set up and install [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac systems. For windows you can use either Docker, VMware, or even [Windows 10 Bash on Ubuntu](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) to install uWebSocketIO. Please see [this concept in the classroom](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/16cf4a78-4fc7-49e1-8621-3450ca938b77) for the required version and installation scripts.

## Run the project
Once the install for uWebSocketIO is complete, the main program can be built and run by doing the following from the project top directory.

    1. mkdir build && cd build
    2. cmake .. && make
    3. ./UnscentedKF

Then open the simulator and click on start.

## Results

### Final RMSE Values:

        rmse_x:  0.0647
        rmse_y:  0.0836
        rmse_vx: 0.3350
        rmse_vx: 0.2193
        
![](images/Finished_loop.png) 

## Workflow

![](images/Workflow.png)

### Basic Idea

The Unscented Kalman Filter solves the problem of finding the best approximation for a Normal Distribution using Sigma Points.
Sigma Points serve as a representation of the whole distribution and we chose them aroung the mean state and in a certain relation to the standard deviation of every state dimention.
After having calcuated the sigma points, we just project them one by one through the non linear function f.

### Prediction Steps
1. **Generate Sigma Points:** we generate an _"augmented"_ mean vector x and covariance P in order to take into consideration the process noise.
`See file src/ukf.cpp from line 159 to line 187`

2. **Predict Sigma Points:** We simply insert evert sigma point into the process model.
`See file src/ukf.cpp from line 196 to line 237`

3. **Predict Mean and Covariance Matrix**: This is where the prediction procedure ends: we need to use the predicted sigma points to calculate the mean and covariance of the predicted state. It is important to notice the use of the weights for each sigma point: because we used lambda before to set how much we want to spread the sigma points, now we need to consider weigh the prediction according to the spread. `See file src/ukf.cpp from line 247 to line 261`
