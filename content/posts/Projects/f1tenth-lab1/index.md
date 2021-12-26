---
title: My F1TENTH Journey — Lab 1, Introduction to ROS
date: 2021-01-24 15:56:44
description: F1Tenth Course Lab 1
theme: Toha
hero: /images/posts/f1tenth-lab1/f1tenth-lab1.png
menu:
  sidebar:
    name: F1Tenth Course Lab 1
    identifier: projects-f1tenth-lab1
    parent: projects
    weight: 10
tags: ["F1Tenth", "F1Tenth Racing", "Multi-lingual"]
categories: ["Projects"]
---

***This series of blogs marks the journey of my F1/10 Autonomous Racing Cars.***

***All my source codes can be accessed [here](https://github.com/shineyruan/F1Tenth_Labs).***

<!-- more -->

## Overview
In Spring 2021 I finally got a chance to join the F1/10 Autonomous Racing Car Lab at Penn, supervised by [Prof. Rahul Mangharam](https://www.seas.upenn.edu/~rahulm/). To start my autonomous racing journey, I followed the [official tutorials](https://f1tenth-coursekit.readthedocs.io/en/stable/index.html) and studied the labs & projects there. 

ROS (Robotics Operating System), is a robotics project development platform widely-used all across the world. Although it is called an "operating system", it is actually a chain of build tools that can be used to build mixed C++/Python projects as well as provide nice & convenient communication functionalities between processes. F1/10 Autonomous Racing Car dev stack is built upon ROS and C++. **Lab 1 also mainly focuses on introducing ROS as well as providing some small hands-on exercises for ROS coding.**

## Lab Materials
The lab specs can be access [here](https://f1tenth-coursekit.readthedocs.io/en/latest/assignments/labs/lab1.html). A copy of PDF is also shown [here](/files/f110_lab1.pdf).

{{< embed-pdf url="/files/f110_lab1.pdf" >}}

The lab was built on the *F1Tenth Simulator*, which can be accessed [here](https://f1tenth.readthedocs.io/en/stable/going_forward/simulator/sim_install.html).

## Workspaces and Packages
This section mainly uses the official ROS tutorials to create a Catkin workspace and a ROS package. I followed the tutorials in ROS C++, set up my workspace, and created a package.

### Answers to Section 3 Written Questions
- **What is a CMakeList? Is it related to a make file used for compiling C++ objects? If yes then what is the difference between the two?**

A CMakeList is a script offering instructions on how to build a CMake workspace. With a proper CMakeList, we can use CMake to build the workspace by 
```bash
cmake ..
make && make install
```
As for its relationship to a Makefile, CMakeLists can be viewed as a *high-level wrapper* of Makefile. In Unix systems, a CMakeList will produce a Makefile.

- **Are you using CMakeList.txt for Python in ROS? Is there a executable object being created for Python?**

CMakeList for Python in ROS does not compile anything. It just imports Catkin macros and copies the corresponding Python scripts to the `bin` folder, if necessary.

- **In which directory would you run `catkin_make`?**

`catkin_make` should be run in Catkin workspaces, aka. `catkin_ws`.

- **The following commands were used in the tutorial**
```bash
source /opt/ros/kinetic(melodic)/setup.bash
source devel/setup.bash
```
**Why do we need to source `setup.bash`? What does it do? Why do we have to different `setup.bash` files here and what is the difference?**

`/opt/ros/kinetic(melodic)/setup.bash` provides necessary Catkin commands & env variables across workspaces, such as `catkin_make`, `catkin_init_workspace`, etc. `devel/setup.bash` provides workspace-specific environment variables, such as the Python libraries it builds, the ROS packages it creates, etc.

## Publishers and Subscribers
In this section I wrote a simple subscriber to subscribe the LiDAR message broadcasted from F1Tenth simulator. The ROS message was a `sensor_msgs/LaserScan` it was published on channel `/scan`.

What's more, I also processed the LiDAR data and found out its maximum/minimum range through a simple linear search, and published the messages on `/closest_point` and `/farthest_point` with data type `std_msgs::Float64`. I also tried to wrap up the processor in a C++ class. The entire code is shown below.

```cpp
#include <ros/ros.h>
#include <sensor_msgs/LaserScan.h>
#include <std_msgs/Float64.h>

class LidarProcessor {
public:
    // Read-only public member variable of closest/farthest range
    //  within a LaserScan
    const std_msgs::Float64& range_closest;
    const std_msgs::Float64& range_farthest;

    // constructor
    LidarProcessor() : range_closest(_closest), range_farthest(_farthest) {}

    // callback function for LaserScan subscriber
    void handleLaserScan(const sensor_msgs::LaserScanConstPtr& new_scan) {
        std::vector<float> ranges = new_scan->ranges;

        // find the min/max range from linear search
        float range_closest = std::numeric_limits<float>::infinity();
        float range_farthest = 0.f;
        for (const float& r : ranges) {
            if (std::isnan(r) || std::isinf(r)) continue;
            if (r < range_closest) range_closest = r;
            if (r > range_farthest) range_farthest = r;
        }

        // clip the data if it exceeds the required min/max range
        _closest.data = (range_closest < new_scan->range_min) ? new_scan->range_min : range_closest;
        _farthest.data = (range_farthest > new_scan->range_max) ? new_scan->range_max : range_farthest;

        ROS_INFO("LaserScan message received: closest %f, farthest %f", _closest.data, _farthest.data);
    }

private:
    std_msgs::Float64 _closest;
    std_msgs::Float64 _farthest;
};

int main(int argc, char** argv) {
    ros::init(argc, argv, "laser_scan_subscriber", ros::init_options::AnonymousName);
    ros::NodeHandle nh;
    ros::Rate loop_rate(10);

    LidarProcessor processor;

    ros::Subscriber laserScan_subscriber = nh.subscribe("/scan", 1, &LidarProcessor::handleLaserScan, &processor);
    ros::Publisher closestPoint_publisher = nh.advertise<std_msgs::Float64>("/closest_point", 1);
    ros::Publisher farthestPoint_publisher = nh.advertise<std_msgs::Float64>("/farthest_point", 1);

    while (ros::ok()) {
        closestPoint_publisher.publish(processor.range_closest);
        farthestPoint_publisher.publish(processor.range_farthest);

        loop_rate.sleep();
        ros::spinOnce();
    }

    return 0;
}
```

### Answers to Section 4 Written Questions
- **What is a nodehandle object? Can we have more than one nodehandle objects in a single node?**

A `ros::NodeHandle` is an extra layer in C++ that provides RAII-style startup & shutdown of the ROS node. It also offers extra functionalities for the node, such as sending/receiving ROS messages, waiting on a specific ROS message, etc. We can indeed have multiple `NodeHandle`s in a single node for creating different namespaces, but they all refer to the same ROS node.

- **What is `ros::spinOnce()`? How is it different from `ros::spin()`?**

`ros::spinOnce()` iterates the loop once more while keeping the node active, while `ros::spin()` puts the entire node into sleep and only handles data from callback functions. 

- **What is `ros::rate()`?**

`ros::Rate` specifies the rate of spinning loop of this ROS node.

## Implementing Custom Messages
In this section I focused on implementing a custom ROS message type `scan_range`, which includes the min/max range in the laser scan. According to the [tutorials of creating custom ROS messages](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv), I created the following ROS message `scan_range.msg`:
```text
std_msgs/Header header
std_msgs/Float64 range_min
std_msgs/Float64 range_max
```
After resolving all Catkin package dependencies in CMakeList & ROS package manifest file (`package.xml`), I finally reached the following output.
```bash
> rosmsg show lab1/scan_range
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
std_msgs/Float64 range_min
  float64 data
std_msgs/Float64 range_max
  float64 data
```

### Answers to Section 5 Written Questions
- **Why did you include the header file of the message file instead of the message file itself?**

According to ROS mechanisms, any customized ROS messages will be generated into a templated message header file to be included in the C++ code. The message file itself is not part of standard C++ and it cannot be parsed by C++ compilers.

- **In the documentation of the LaserScan message there was also a data type called Header. What is that? Can you also include it in your message file? What information does it provide? Include Header in your message file too.**

The `Header` provides information on the sequential order, the timestamp, and the frame ID of the instant when the message is published. It is very useful when one would like to make use of time information of the messages received. The header file has been included in my custom ROS message.


## Recording and Publishing Bag Files
This section aims at playing with `rosbag` and bag files in ROS. ROS bag files are a certain kind of files that targets at recording ROS messages, so that it can be replayed elsewhere afterwards. ROS Bag is extremely helpful when one would like to collect data in real-world experiments. 

### Answers to Section 6 Written Questions
- **Where does the bag file get saved? How can you change where it is saved?**

By default the bag file is saved at the current directory. We can also change the directory by adding `-o` such as:
```bash
rosbag record -o ../bagfiles/my_rosbag_recordings.bag
```

- **Where will the bag file be saved if you were launching the recording of bagfile record through a launch file. How can you change where it is saved?**

By default the bag file is saved at the current directory. We can also change the directory by appending `args=../bagfiles/my_rosbag_recordings.bag` to the corresponding node in the launch file.

## References
F1/10 Autonomous Racing Lecture: Course Introduction

{{< youtube zENhppcxwzY >}}

F1/10 Autonomous Racing Lecture: Using the Simulator

{{< youtube Q71OKxztppA >}}



