# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program


---
[//]: # (Image References)

[image1]: ./Result/update_equation.png "Update Equation"
[image2]: ./Result/constraint.png "constraints"
[clip1]: https://youtu.be/QtXPXIw2QBs "Complete_Lap"


---

* Source Code can be found [here](https://github.com/hassmuha/CarND-MPC-Project-Submit/tree/master/src)
* Compilation Result ./particle_filter can be found [here](https://github.com/hassmuha/CarND-MPC-Project-Submit/tree/master/build)
* Video Clipping for the simulator results can be found [here](hhttps://github.com/hassmuha/CarND-MPC-Project-Submit/tree/master/Result)

---

### Key Points

The MPC Controller project has been developed in the same way as it was taught in the lecture. Summary of the implementation has been discussed below

* We have used kinematics model with state variables as shown below
state = [x, y, psi(car orientaiton) , v(velocity), cte(Cross Track Error), epsi(Orientation Error)].
The kinematics Update model equation are ![alt text][image1]
As we initially make the the psi negative before converting the waypoints to vehicle coordinate system (main.cpp:line99). Hence there is no need to change the psi update explained as shown in the tips of the equation. The relevant code can be found (MPC.cpp:line130)
As an actuation commands, we have steering angle and throttle value. These values for each step is calculated by predicting trajectory in 3rd order equation along with minimizing the cost function. Actutor constraints have been added as well which are
![alt text][image2]

* Final values of N (timestep length) and dt (elapsed duration between timesteps) selected are N = 25, dt = 0.05 respectively. Before that different combinations have been tried for example dt = 0.1 and 0.25 with same value of N. Moreover N has been changed to from 15 to 25 with discrete step of size 5. In all of the combination final selected combination works always better.  
Moreover for the cost function (MPC.cpp:line59) different weights have been used also as hyperparameters and tried different combinations. Final weights for the costs are: 1, 10, 1 for the cost based on the reference states, 10, 5, for the minimizing actuators usuage and 500, 50 for minimizing the rate of change of actuation. These values were chosen to minimize orientation errors as maximum as possible, punishing lot for variation in steering angle, provide smoothness in steering transitions and keeping the car acceleration also stable.

* 3rd order polynomial is used to fit on waypoints. But before applying the polynomial fitting routine waypoints have been converted to vehicle coordinate system (main.cpp:line111). Also reflection factor for y-coordinate and psi has been applied, as y-coordinate becomes positive when moving top down in simulator.

* The solution to the 100ms latency for MPC has been catered for in line code MPC.cpp:line273. The idea is based on values of dt and sending the simulator the actuation commands on the predicted trajectory mapping to 100ms infront of car.

* Final Result Video [here][clip1]
