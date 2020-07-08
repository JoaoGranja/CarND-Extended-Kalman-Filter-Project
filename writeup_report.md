# **Extended Kalman Filter** 

---

**Extended Kalman Filter Project**

The goals / steps of this project are the following:
* Implement an extended Kalman filter in C++ to track a car's position and velocity
* Test the Kalman filter performance using a simulator


## Writeup Report

In this report I will address all steps of this project, explaining my approach and presenting some results obtained.

---
### Step 0 - Implement an extended Kalman filter in C++ to track a car's position and velocity

I used the base code provided and filled the following missing functions code: 
1 - In tools.cpp, I filled the functions that calculate the root mean squared error (RMSE) and the Jacobian matrix. On Jacobian matrix calculation, the code is protected against dividing by zero. In that case, a zero matrix is returned. 

2 - In FusionEKF.cpp, I initialized the Kalman Filter parameters in the constructor method. In 'ProcessMeasurement' function, I initialized the state vector (P_ and x_) with the first sensor measurement. For radar measurement, I used the range_rate observation to initialize the vx and vy states assuming that the perpendicular velocity related with range vector is zero.
In 'ProcessMeasurement' function, I implemented the processing flow: 1- calculate elapsed time, modify the F and Q matrix so that the time is integrated; 2- predict the next state; 3- update the state based on the observations from the sensor measurement.

3 - In kalman_filter.cpp, I filled out the Predict(), Update(), and UpdateEKF() functions.

### Step 1 - Test the Kalman filter performance using a simulator

I tested the Extended Kalman Filter algorithm with the simulator and for both Datasets 1 and 2, I obtained a RMSE lower than [.11, .11, 0.52, 0.52].

Using the flag variables 'use_Laser' and 'use_Radar' I could observe how the estimations change when running against a single sensor type. The final results obtained for RMSE are:

|	Dataset	|		Laser and Radar		|		Laser Only			|		Radar Only		| 
|:---------:|:-------------------------:|:-------------------------:|:---------------------:| 
|			|		x  - 0.0973			|		x  - 0.1473			|	x  - 0.2302			|
|	1		|		y  - 0.0855			|		y  - 0.1153			|	y  - 0.3464			|
|			|		vx - 0.4513			|		vx - 0.6383			|	vx - 0.5835			|
|			|		vy - 0.4399			|		vy - 0.5346			|	vy - 0.8040			|
|:---------:|:-------------------------:|:-------------------------:|:---------------------:| 
|			|		x  - 0.0740			|		x  - 0.1211			|	x  - 0.2706			|
|	2		|		y  - 0.0964			|		y  - 0.1267			|	y  - 0.3852			|
|			|		vx - 0.4466			|		vx - 0.6274			|	vx - 0.6715			|
|			|		vy - 0.4756			|		vy - 0.5989			|	vy - 0.9411			|

During the simulation, it was clear for all cases that the RMSE start with high values which decreased over time. From the table above, it is possible to conclude that the laser measurements are more accurate than radar (as expected). Using both measurements, I obtained better and accurate results because we gain more certainty.