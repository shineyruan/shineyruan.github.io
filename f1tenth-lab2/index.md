# My F1TENTH Journey — Lab 2, Automatic Emergency Braking


***This series of blogs marks the journey of my F1/10 Autonomous Racing Cars.***

***All my source codes can be accessed [here](https://github.com/shineyruan/F1Tenth_Labs).***

**Previous post:**

- [My F1TENTH Journey — Lab 1, Introduction to ROS]({{< ref "/posts/f1tenth-lab1/index.md" >}})

<!-- more -->

## Overview

This lab focuses on implementing an AEB (Automatic Emergency Braking) in ROS node for F1/10 racing cars. AEB is widely used in autonomous vehicles as a basic safety guarantee to avoid collisions with objects.

The lab materials can be accessed [here](https://f1tenth-coursekit.readthedocs.io/en/stable/assignments/labs/lab2.html#). A PDF version is also attached [here](/files/f110_lab2.pdf).

The lab was built on the *F1Tenth Simulator*, which can be accessed [here](https://f1tenth.readthedocs.io/en/stable/going_forward/simulator/sim_install.html).

## Demo

{{< youtube vVHXqJv6NbY >}}

## Time-To-Collision

TTC (Time-To-Collision) is the time it would take for the vehicle to collide with an obstacle given its current heading and velocity. TTC can be calculated with the following format:

$$ TTC = \frac{r}{[-\dot{r}]_+} $$

where $r$ is the distance between vehicle and obstacle, $\dot{r}$ is its 1st derivative with respect to time, and the symbol $[x]_+$ denotes $\max(0,x)$.

In practice, we use LiDAR results to calculate TTC for each beam. Specifically, we project the current vehicle velocity onto the direction of each beam as $\dot{r}$, namely $\dot{r}=v\cos(\theta)$.

`sensor_msgs/LaserScan` provides us with LiDAR beams in all directions. In each `LaserScan` message, we have `angle_min`, `angle_max` and `angle_increment`, which corresponds to the *starting angle*, the *ending angle*, and the *step angle* between two adjacent laser beams. All the angles are given in the local coordinate frame (positive $x$ direction is the vehicle's heading) and positive $x$ direction is marked as angle value $0$.

We also have `float32[] ranges` in each `LaserScan` message, which gives us the distance to obstacle in the corresponding direction angle. Namely, `ranges[i] <--> angle_min + i * angle_increment`.

Given the above information, it is not difficult for us to write our AEB ROS node.

## References

F1/10 Autonomous Racing Lecture recordings:

{{< youtube jZR3tk9IWlY >}}

