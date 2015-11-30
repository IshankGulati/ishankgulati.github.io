---
layout: post
title: "Caretaker Robot"
date: 2015-11-30
comments: True
tags: Robotics Artificial-Intelligence Computer-Vision
---

## Introduction

This project is a part of e-yantra+ Robotics Competition (eYRC+), a unique annual competition for undergraduate students in science and engineering colleges organised by IIT Bombay. We were given a robotic kit, consisting of Firebird V robot and other components required to complete the task.

e-Yantra Robotics Competition Plus has designed a theme to bring our awareness to the issue of catering to the needs of patients, for example, automating the task of serving them, even in the absence of human supervision.

In this theme, the arena is an abstraction of a floor in a hospital, which includes a Corridor, a Patient zone and a Service zone. The robot is informed of the patients’ requests via a computer that processes the images from a camera and it autonomously seeks the provisions to be picked up and delivered to the corresponding patients.

My team had to design the algorithm for 

* Detection of provisions requested by the patients.
* Matching them with provisions at the Service zone.
* Delivering them to the patients in the Patient zone.

## Theme Description

1. The arena design is as shown in Figure 1:
* There are two zones in the arena: (i) Patient zone and (ii) Service zone, connected by a Corridor.
* There are three types of provisions that a patient can request: Medicine, Water, or Thermometer.
* These provisions are represented by Provision markers of different colors:
	+ Red (R) for Medicines
	+ Blue (B) for Water, and
	+ Yellow (Y) for Thermometer.
* The Patient zone consists of:
	+ Three beds for patients: BED1, BED2, BED3
	+ Each patient has a table: T1, T2, T3
	+ Provision markers (R, B or Y) will be placed on the tables to indicate request for a provision.
* The Service zone consists of:
	+ Three storage cupboards marked by positions, P1, P2, P3.
	+ Each storage cupboard (P1, P2, or P3) contains one type of provision: Medicine, Water, or Thermometer, marked by a corresponding Provision marker.
	+ P1, P2, P3 are separated by Fixed Partitions, FP1 and FP2, as shown in Figure 1.
* The Corridor is made of wall divisions placed at positions marked by D1, D2, .. , D8.
* There are 4 wall divisions – 2 on either side of the corridor – placed at 4 specified positions among D1, D2, .. , D8. Placements of the wall divisions will be provided as input. An example is shown in Figure 1. Note that the vacant divisions will be used by the robot to navigate between the two zones.
* There are 3 possible Start positions, S1, S2, or S3, for the robot, marked on the corridor.

2. There is an overhead camera that captures the entire arena and is above the center of the arena.

3. The camera is connected to a computer that uses Python and OpenCV to analyze the image obtained by the camera in order to detect the locations of the Firebird V robot and compute the path to be followed. This information is conveyed to the robot by using a pair of Zigbee modules, one connected to the robot, and the other connected to the computer.

4. Since the emphasis is on image processing, the robot need not literally pick up the provisions, but instead indicate the same by use of 2 RGB LEDs provided in the kit.

5. The robot may carry either zero, one or two provisions at the same time. In order to indicate the same, the teams must use 2 RGB LEDs and display them prominently on the robot.

6. In order to indicate that it is carrying a provision, it must turn on the appropriate color of LED:
* Medicines – Red
* Water – Blue
* Thermometer – Yellow
* For example, if the robot is carrying 1 thermometer and 1 medicine, it must turn on the two RGB LEDs, one showing yellow and one showing red. If it is carrying 2 glasses of water, it must turn on both RGB LEDs showing the color blue. If it is carrying only 1 glass of water, it must turn on only one RGB LED showing the color blue. If it is carrying nothing, it must turn all LEDs off.

7. The robot starts from a specified Start position in the Corridor and does the following:
* Detects the positions of the wall divisions, (ii) the provisions requested by the patients in the Patient zone and (iii) the positions of the corresponding storage cupboards in the Service zone.
* Moves to the appropriate storage cupboard/s in the Service zone.
* When the robot has reached a storage cupboard, it can indicate the pick-up of a provision by turning on an RGB LED with the color corresponding to the provision contained in that storage cupboard. The robot can only pick up a provision if it is presently carrying either none or one provision.
* When the robot has to deliver a provision to the patient, it must move close to the appropriate table, and indicate it has delivered the appropriate provision by turning off the RGB LED with the color corresponding to the provision requested by that particular patient. The robot can only deliver a provision if it is presently carrying either one or two of that provision.

8. Once the robot has delivered all the requested provisions to the patients, it must indicate the end of the run by sounding a continuous buzzer.

<center><img src="/assets/arena.PNG" alt="eyrc arena" style="width:600px;height:400px;"/></center>


>**Note:** This project primarily uses colour based object segmentation and A* algorithm for path planning. Make sure to revise these topics before proceeding. You can refer to [this](http://ishankgulati.github.io/2015/11/26/Pathfinding%20using%20Astar%20Algorithm/) and [this](http://ishankgulati.github.io/2015/11/23/The%20Pool%20Game/) post for introduction to these topics.

## Algorithm

* The image taken by camera is divided into a grid, where origin is taken as upper-left corner. Anytime we call A* for path planning, it will return the path in terms of coordinates of this grid.

* Since all the obstacles, markers etc are of distinct colours they can be easily segmented out by masking and the finding the contours. Moments method can be used to find out centroid of the markers. This can be translated into the gridwise coordinate system we have adopted. Also spatial properties of image can be used to differentiate between service zone(left) and patient zone(right). So, In this step the coordinates of all the markers and obstacles are calculated and stored corresponding to their colour.

* Mark the position of all the obstacles and markers in a matrix of the size of grid which will be again used in exexuting A*. 

* To easily locate the bot, I fixed two distinct coloured stickers on top of it. Let's denote the front sticker by F and rear sticker by R. To find out the coordinates of bot, simply apply above step on these stickers. To detect orientation of bot:
	+ North - distance of F < distance of R from upper side of arena, distance of F = distance of R from left side of arena
	+ South - distance of F > distance of R from upper side of arena, distance of F = distance of R from left side of arena
	+ East - distance of F > distance of R from left side of arena, distance of F = distance of R from upper side of arena
	+ West - distance of F < distance of R from left side of arena, distance of F = distance of R from upper side of arena

* In the next step, calculate the number of markers of given three colours in patient zone. Further algorithm can be divided into three broad cases:
	1. All the markers are of same colour.
	2. Two markers of same colour.
	3. All the three markers are of different colours.

I will explain the first case where all the markers are of same colour.

#### Three markers of same colour
	* Since all the markers are of same colours, A* algorithm is used to calculate path from it's initial position to the corresponding coloured marker in Supply zone. 
	* Indicate pick up by glowing the corresponding coloured LEDs.
	* Again use A* to calculate path from this marker to all the other markers in Patient zone and then choose the path with least path length.
	* After delivering to the first marker in patient zone, move to the nearest marker and switch off the second LED.
	* From there again use A* to determine the path to respective coloured supply.
	* Pick up the last supply and deliver it to the leftout marker by same procedure as given above.
	* Sound the Buzzer.

* All other cases are similar in my implementation. I have computed distance from all the markers from current position anf then used greedy approach to select the one with minimum path length.

> Refer my [repository](https://github.com/IshankGulati/Caretaker-Robot) for complete code.

Here is the robot in action

<a href="http://www.youtube.com/watch?feature=player_embedded&v=1ifXxdzdyec
" target="_blank"><img src="http://img.youtube.com/vi/1ifXxdzdyec/0.jpg" 
alt="Line follower" width="640" height="427" border="5"/></a>

Thanks! 