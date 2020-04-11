---
layout: post
title:  "Robotics: obstacle avoidance exercise"
description: "My obstacle avoidance proyect in Phyton"
last_modified_at: 2020-02-17
image:
categories: Robotics, Technology
tags: [Robotics, Phyton, cv2, VFF]
author: dhrodao
---

# 1. Introduction
The objective of this exercise is to complete the entire circuit avoiding the obstacles that the car will find on its trajectory.
For solving this logic problem its required the implementation of an VFF Local Navegation algorithm, wich will be detalled later.

<figure class="align-center">
  <img src="{{ '/assets/images/blog/p3.png' | absolute_url }}" alt="Figure 1. Scenario.">
</figure>


# 2. Hardware
## 2.1. Sensors
The sensor that we have to carry out the exercise its a laser sensor (<em> Lidar </em> sensor) that takes measures from 0º to 180º on the front side of the car. We will use this sensor to locate the obstacles.

## 2.2. Actuators
As actuators, the robot has motors, those motors let the car move on every direction.

# 3. Software
Firstly, to carry out the VFF algorithm is used a hybrid navegation, so, the API is the one who provides us the global coordinates of the targets the car needs to reach. There is many targets along the circuit that helps the car to complete the lap.

This algorithm is an iterative algorithm whose structure is as detailed:

<p>
    · Firstly, we need to obtain the absolute coordinates from the robot (x, y, yaw), where yaw makes reference to the orientation angle.
</p>
<p>
    · Obtain the absolute coordinates from the next target.
</p>
<p>
    · Calculate the relative coordinates from the objective (regarding my robot) from the previous absolute coordinates.
</p>
<p>
    · Obtain the sensor data. This data is received in a 180 position array ([0,179]) where each position contains a distance value.
 </p>
 <p>
    · Once we have the relative coordinates from the target and the sensor values, the VFF algorith is applied.
  </p>

# 4. VFF algorithm
This is a local navegation algorithm that is based on the calculation and adding of forces to calculate the optimal direction and sense for the robot to navegate safely.

## 4.1. Atractive force
With the X and Y coordintes from the target, we define an force, that will be composed by module and phase. The module need to be constant in order the force doesn't grows uncontrollably.

## 4.2. Repulsive force
Firstly, we transform each angle of measuring from the sensor (0º to 180º) to the range [-90º,90º]. This is obtained by subtracting 90º to the initial angle. Lately it will be added by 180º. This is done because we want a repulsive force, so we need to invert it. Once we now the phase from the repulsive force, we calculate the X and Y coordinates (for every position of the array) using the distance measure as is detailed:

<pre>
    dist_threshold = 4
    vff_repulsor_list = []
    for dist,alpha in laser:
        if(dist < dist_threshold):
            x = -math.cos(alpha)/d
            y = -math.sin(alpha)/d
            v = (x,y)
            vff_repulsor_list += [v]
</pre>
Where <em> alpha </em> is the phase from the repulsive force. On this loop what we are doing is saving in an array (<em> vff_repulsor_list</em>) al the measure vectors from the laser data. And finally we need to convert al this vector in one:

<pre>
    vff_repulsor = np.mean(vff_repulsor_list, axis=0)
</pre>

## 4.3. Resultant force
Finally, once calculated the atractive and repulsive forces, it's neccesary to combine both to get the resultant vector wich will provide us the information that we need to send information tho the motors in order to get the car moving and avoiding the obstacles.
Firstly, we must add the X and Y coordinates from the two initial forces. This values will be the coordinates from the resultant vector:
<pre>
    self.avgx = self.carx + self.obsx
    self.avgy = self.cary + self.obsy
</pre>
Lately, we calculate the module and phase from this vector and done with the other vectors.

# 5. Visual example
This is the first version of my exercise, the problem is tha the car oscilates too much so I need to correct it on the next version:

<pre>
  <div class="video-responsive">
    <iframe src="https://www.youtube.com/embed/bNhEaRjoX08" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</pre>


I continued improving my vectors interpretation and some other aspects like a case based method for the steering angle and these are the results:

<pre>
  <div class="video-responsive">
    <iframe src="https://www.youtube.com/embed/A17T6BaYDo8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</pre>

The next version of the exercise was tested on the offline version of the platform. I made some changes, this is the visual example:

<pre>
  <div class="video-responsive">
    <iframe src="https://www.youtube.com/embed/g-R8jsmUkdY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</pre>

As you can see on the previous version, the vectors weren't correctly displayed so I fixed that issue and some others related with the obstacle vector, also the car isn't that agressive now:

<pre>
  <div class="video-responsive">
    <iframe src="https://www.youtube.com/embed/506_BLl2cLE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</pre>
