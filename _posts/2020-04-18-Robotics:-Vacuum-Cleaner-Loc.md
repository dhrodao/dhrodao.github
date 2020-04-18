---
layout: post
title:  "Robotics: Vacuum Cleaner"
description: "My Vacuum Cleaner proyect in Phyton"
last_modified_at: 2020-02-17
image:
categories: Robotics, Technology, AutoLoc
tags: [Robotics, Phyton, AutoLoc]
author: dhrodao
---

# 1. Introduction
The objective of this exercise is to program the intelligence of a vacuum cleaner in order it autolocates in a house (with the given map). The map is a binary image, and you need to process it to abstract information. The objective is to cover the largest area of the house using the auto location algorithm.
<figure class="align-center">
  <img src="{{ '/assets/images/blog/vacuum_cleaner.png' | absolute_url }}" alt="Figure 1. Scenario.">
</figure>

# 2. Hardware
The vacuum cleaner is equipped with:
<ul>
  <li>A pair of motors for moving</li>
  <li>Laser</li>
</ul>
I did not use the laser in order to solve the exercise.

# 3. Conversion of types
In order to get the 3d coordinates from Gazebo equivalent to our map we need to apply an scale, in my case I resized the image to 500x500px and stablished a scale of 50px per meter. The matrix of conversion of types is the following:
<figure class="align-center">
  <img src="{{ '/assets/images/blog/matrix.png' | absolute_url }}" alt="Figure 2. TF Matrix.">
</figure>
With this conversion of types we can stablish the position of the vacuum on the gazebo scenario.

# 4. Route planification
