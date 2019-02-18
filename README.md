# CarND-Path-Planning-Project
### Overview
The goal of this project is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. The simulator provides the car's localization, sensor fusion data, and map  waypoints. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible. The car should avoid hitting other cars and stay in lanes but changing lanes.  The acceleration shall be less than 10 m/s^2 and jerk less than 10 m/s^3.

My car can make one complete loop without incident, following is a screenshot.

![1](/screenshot/1.jpg)

### Reflection

I started with the code from walkthrough. I changed the acceleration to 0.224 * 2 firstly at line 370 - 376, which helps the car to get to the 50 MPH speed limit faster and also slow down faster to avoid collision when following too close. Then I added change lane left and right, which helps the car runs further like following.

![CL](/screenshot/CL.jpg)

But there's still some violation of max acceleration when changing lanes. So I removed the acceleration when changing lane at line 374. Then it can make one complete loop without incident.

1.From line 298 to line 367 is State machine and cost calculation.

This code segment checks car's state and environment (sensor fusion data). It detects if the previous car is too close (line 312 - 323), and whether there's a car in the right/left lane making a lane change is not safe (line 171- 206). The safe_change_lane function checks the target lane, if the car's position at the end of the future trajectory is close to other car (20 meters, in front or behind), the lane change is considered unsafe (cost is 1).

2.From line 369 to line 471 is behave and trajectory generation.

This code segment accelerates the car or slows down it when following too close (line 369 - 376), changes lane (line 415 - 417), and generates the trajectory. 

The code combines the last points of the previous path and three points at 30/60/90 meters with the spline library calculation to fit the trajectory. The rest of the points are calculated by feeding the x values to spline with 0.02 intervals (line 456 - 474). 

### Extra

Furthermore, I also add a code snippet to only change lane when the car in target lane moves faster (line 195 - 211), and it can be switched on/off by parameter watch_speed. With this feature turned on, the car can run through longer distance without incidents. The longest one I tried is 70 miles like following screenshot.

![3](/screenshot/3.jpg)

### Enhancement

It'll be better to organize the code into separate functions, such as get_successor_state(), generate_trajectory(), evaluate_trajectory_cost(), and so on. So that the new state like preparing lane change can be incorporated easily. Adding flexible and finer control, such as changing acceleration according to the distance with previous car, will also be easier by manipulating the cost functions.   