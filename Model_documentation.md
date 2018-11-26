# Path Planning Project
This project implements the path planner for an autonomous car in Highway. Highway has fewer constraints than urban driving. But it is a good case study for variety of use cases like keep lane, lane change left, lane change right, collision avoidance, speed maintenance etc.

## High level Design
The path planner receives the map, senser fusion data(with infomation on the other cars in Highway) and the autonomous car's localization data as input.

The simulator accepts a set of x and y values (50 each) and uses those points to drive the car. It visits each of the points in successive intervals of 0.02 seconds. This is used to mention the acceleration to the car by adjusting the corresponding xy positions in the path planner. It would use only a few points before it receives the next path points from the path planner. The simulator always feeds back the previous coordinates that are not yet driven. This will be used by the path planner as part of the next set of coordinates. This results in a smoother curve.

To generate a better smooth curve for the path planner, 3 evenly spaced waypoints(30 waypoints in our case) are created and added to the previous points returned by the simulator. These points are fed to spline (https://kluge.in-chemnitz.de/opensource/spline/) to generate a smooth curve. Spline guarantees a smooth curve and to pass through all the points. 

The path planner keeps track of the vehicles that are too close ahead and behind the autonomous car in each of the lanes. If the car in front of us in the same lane is too close, it attempts to change the lane when it is safe to do so i.e. when there are no cars close to the autonomous car in the next lane. First preference is given to the fastest lane(Lane - 0) and if it is not safe then the next preference is to change to lane 2. If the lane change is not possible, the car attempts to slow down the acceleration to maintain safe distance.

When the reference velocity is less than the speed limit, the path planner gradually increases the acceleration.

Most of the calculations are done using Frenet coordinates to simplify the math and finally converted to Cartesian coordinates.

## Write up below highlights each of the Rubric Points and explains how the solution meets the criteria.
### The car is able to drive at least 4.32 miles without incident.
The car is able to drive without incident for multiple laps without incident. Attached images shows the best miles and current miles without incident to be the same value. The lane change logic and acceleration change logic define between lines 296 to 324 avoids many incidents.

### The car drives according to the speed limit.
Reference velocity of 49.5 mph is used in line 322 to 324. This ensures acceleration to never exceed this reference velocity. This reference velocity is also used in line 423 to calculate the x and corresponding y points.

### Max Acceleration and Jerk are not Exceeded.
These 2 are avoided by gradually increasing or decreasing the acceleration(line 299 to 324) and smoothening the lane changes using spline.

### Car does not have collisions.
Collision is avoided by always checking vehicles that are close in the current lane and either changing to a lane when it is safe to do so or slowing down(lines 296 to 324).

### The car stays in its lane, except for the time between changing lanes.
Lane change is attempted only when it is required per the logic defined in the high level design. The car remains within the lane at all other times and the d value of the car is defined accordingly.

### The car is able to change lanes
Lane change is already explained above.
