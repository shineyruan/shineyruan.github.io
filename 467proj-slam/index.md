# SLAM — Simultaneous Localization And Mapping


Simultaneous Localization And Mapping, also known as SLAM, is a set of very important techniques in robotics, and it's still in active research. It aims at developing different algorithms for figuring out the position of the robot in the real world by the robot itself.

<!--more-->

## Background: What is SLAM?

Think about a scenario where you were led to an unknown building, and you were standing at the beginning of a corridor. Yes, right now you were completely lost, and you wanted to figure out your position in the building. With initially zero information, you now began to look around at the surroundings. Very soon, you acquired a general sense of where you are with respect to all the objects around you. Then you realized that keeping standing at the same place won't offer you more information, and you began to move. While you were moving, you kept checking out the new surroundings, obtained information about your new position, and updated your prior positions with the new information. Finally, you reached the end of the corridor, with a complete local map of the general shape of the corridor in your head. If you kept going, you would obtain more and more information about the new environment and also get your prior information updated. At some point you would be familiar enough with the building to figure out where you were in it.

Although it might seem natural for readers as you to get familiar with an unknown environment by looking and walking around, this process is actually complicated when it comes to applying it on a robot. In the field of robotics, this process is called **SLAM (Simultaneous Localization And Mapping)**. So far, there have been a number of different models that accomplish this task, with one of the most basic model called *Markov Localization*.

In the fall of 2019, I took the course [*Autonomous Robotics (EECS 467)*](https://web.eecs.umich.edu/~kuipers/teaching/eecs467-F19.html), in which I implemented a complete SLAM algorithm with occupancy grid mapping and Markov Localization for a robot car with 3 other teammates [Gregory Meyer](https://github.com/Gregory-Meyer), [Nathan Brown](https://github.com/nlbrown2), and [Martin Deegan](https://github.com/martindeegan).

## Demo Videos

The following is a demo video of our algorithm visualization on a laptop.
{{<youtube zsRtV_YeBbc>}}

The following video is a demo of our robot car running in the unknown environment.
{{<youtube qiB47C4CRBo>}}

