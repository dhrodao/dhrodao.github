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
In order to get the 3d coordinates from Gazebo equivalent to our map we need to apply an scale, in my case I resized the image to 500x500px and stablished a scale of 50px per meter. The matrix for conversion of types is the following:
<figure class="align-center">
  <img src="{{ '/assets/images/blog/matrix.png' | absolute_url }}" alt="Figure 2. TF Matrix.">
</figure>
With this conversion of types we can stablish the position of the vacuum on the gazebo scenario.

# 4. Route planification
In order to calculate the route of the vacuum it will be needed a navigation mesh applied on the house map. The dimension for the cells should be the dimension of the vacuum (16x16px) in order to cover every zone of the house map. 
The map will be used for checking the paths where the vacuum has passed on. Using the cv2 method for erosion we get the obstacles dilated a bit and this is a good point in order the vacuum doesn't hit any obstacle.
Firstly, we need to calculate the cells in wich the vacuum is. And traduce this coordinates to pixels on the map. Once we get the position on the map and created the first cell, it will be calculated adjacent cells to it (on North, South, East and West). The cells could be on one of three values:
<ul>
  <li>Obstacle: At least one of the pixels of the cell is black</li>
  <li>Virtual obstacle: The vacuum has already passed by this cell before</li>
  <li>Free: Every pixel on the cell is white</li>
</ul>
We will need tho check every close cell and check its value. 
With a specific method we will calculate the next cell the vacuum needs to move.

# 5. Return route
