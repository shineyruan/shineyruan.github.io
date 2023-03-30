# My F1TENTH Journey — Lab 6, Pure Pursuit (Waypoint Tracker)


***This series of blogs marks the journey of my F1/10 Autonomous Racing Cars.***

***All my source codes can be accessed [here](https://github.com/shineyruan/F1Tenth_Labs).***

**Previous post:**

- [My F1TENTH Journey — Lab 1, Introduction to ROS]({{< ref "/posts/f1tenth-lab1/index.md" >}})
- [My F1TENTH Journey — Lab 2, Automatic Emergency Braking]({{< ref "/posts/f1tenth-lab2/index.md" >}})
- [My F1TENTH Journey — Lab 3, PID-Controlled Wall Follower]({{< ref "/posts/f1tenth-lab3/index.md" >}})
- [My F1TENTH Journey — Lab 4, Reactive Planning Methods for Obstacle Avoidance]({{< ref "/posts/f1tenth-lab4/index.md" >}})

<!-- more -->

## Overview

In autonomous driving development, recording waypoints for future testing is very important. One way to record waypoints efficiently is only remembering the x, y, z coordinates of the vehicle regularly for some constant time gap. However, as ground vehicles are generally *non-holonomic* (i.e., the vehicle cannot make a turn in place), it is non-trivial to control an autonomous vehicle to exactly follow these pre-recorded waypoints. Hence, we need to design a path tracker (waypoint tracker) so that it can calculate and send commands to the vehicle to make it follow the pre-recorded waypoints. That is how "Pure Pursuit" algorithm comes into place.

## The Pure Pursuit Algorithm

The Pure Pursuit algorithm accounts for the non-holonomic constraints and the speed of the vehicle when it follows the waypoints. Given the current position of the vehicle, what it does is to always steer towards the next appropriate waypoint according to some lookahead distance $L$. In real practice, Pure Pursuit algorithm is often used with localization methods, i.e., a particle filter.

But since the vehicle is non-holonomic, how can we steer towards the next waypoint properly? Setting the exact direction of the next waypoint from the current vehicle position is not feasible, as the vehicle is always in high speed and might overshoot the desired goal. One alternative option tries to interpolate the trajectory by finding an arc between the current position and the next waypoint (goal), which can be showed as the figure below.

{{< figure src="f1tenth-lab6-arc.jpg" caption="Arc interpolation between current position and goal." >}}

However there are infinitely many arcs between two points, and each of them has a different radius (curvature). Therefore we would like to add one more constraint to the arc: its center of radius must lie on the $y$ axis. In such way we can always find a unique arc interpolation for our trajectory, and it can be solved by simple math. After solving the geometry we can get the radius of the arc:
$$r=\frac{L^2}{2|y|}$$

{{< figure src="f1tenth-lab6-steer.jpg" caption="Finding the steering angle from the arc interpolation." >}}

Finally, we can simply set the steering angle proportional to the curvature of the arc:

```cpp
double steering_angle = K_p * curvature;
```

`K_p` works exactly the same as a P controller and its value can also be tuned using PID tuning theory.

In summary, the Pure Pursuit algorithm can be implemented in the following steps:

1. Find the next waypoint (goal) from the current position and the list of all waypoints.
2. Interpolate an arc between current position and goal, and calculate its radius & curvature.
3. Set the steering angle proportional to the curvature.

## Actual Implementation

This section summarizes a list of issues when I was implementing the pure pursuit algorithm in ROS and C++.

1. **Running the particle filter**.

In real practice we can never know the ground truth position of the vehicle. The position of the vehicle can only be measured using some kind of sensors and algorithms. For this lab I made use of a decent particle filter ROS package written by [MIT car racing team](https://github.com/mit-racecar/particle_filter). As I was using ROS Noetic with Python 3 while this package was written in Python 2, I made a fork of their project and spent some time converting their code into Python 3 to fit in my workspace. For future reference, the Python 3 version of this particle filter can be accessed [here](https://github.com/shineyruan/particle_filter).

It is worth mentioning that this particle filter provides several options for laser beam ray computing, and all of them depends on a ray-marching library called [RangeLibc](https://github.com/kctess5/range_libc). The original RangeLibc was also written in Python 2 and I also made [a fork of it](https://github.com/shineyruan/range_libc) and converted it into Python 3. With my modification the entire particle filter pipeline should now be able to run successfully in Python 3 and ROS Noetic.

2. **Choosing the goal waypoint from current position and lookahead distance.**

In general, there are multiple ways to choose the goal waypoint for the vehicle. Some options include choosing the waypoint in the list that has distance closest to $L$ from the current vehicle position, interpolating a waypoint with distance exactly equal to $L$ from the closest waypoint in the range and out of the range, and choosing the waypoint closest to $L$ from those out of the range. As for my implementation, I chose to implement in the following way:

- Starting from the current position, pick the first waypoint that has a distance larger than $L$ from the current position.

Notice that my implementation is current not the best option, as it has not incorporate the current orientation of the car and it might cause troubles when the next goal is somewhere behind the vehicle.

3. **Convert the goal coordinates to local frame before calculating the curvature of the arc.**

As the curvature calculation assumes that our vehicle is always facing in the positive $x$ direction, we must convert the goal waypoint into local frame before calculations.

4. **Use signed curvature for the steering angle.**

In real practice the next waypoint could be either in the front-left or front-right of the vehicle, resulting in the fact that the curvature could either be concave or convex. Hence it is important for us to consider the sign of the curvature so that we can steer our car in the correct direction.

## Demo Videos

The Pure Pursuit algorithm with the vehicle running in 5m/s. Green trajectory is the pre-recorded waypoints; red dot is the goal waypoint for the vehicle; blue dot is the current position of the vehicle (using ground truth position in simulator).
{{< youtube Qs2StzvzXHw >}}

The Pure Pursuit algorithm was also successfully run in the real world. The following experiment was conducted in the 2nd floor of Levine Hall at the University of Pennsylvania.
{{<youtube t9NOFv8BmfY>}}

## References

F1/10 Autonomous Racing Lecture: Pure Pursuit
{{< youtube r_FEKkeN_fg >}}

