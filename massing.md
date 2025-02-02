---
layout: default
title: "Massing"
permalink: /massing
remote_theme: pages-themes/cayman@v0.2.0
---

# Massing
This chapter describes how we translated the concepts and diagrams into a parametric building program using Houdini. The last chapter reported on how the original envelope was created. Now we are gonna talk about how different analyses were done to improve the envelope. The last part of the massing chapter talks about allocating functions to their analysed locations. The following flowchart can be used as a reference for the algorithm in Houdini.

<div style="text-align: center;">
  <img src="assets/algorithmFlowchart.png" alt="Algorithm flowchart" style="width: 700px;">
  <p><em>Figure 3.1: Algorithm flowchart</em></p>
</div>

## Analyses
Now that we have an envelope, we want to inhabit this envelope with different functions. But how to decide where to place each function? This is done by thoroughly analysing the current envelope. Is there enough sunlight in my house? How much noise does my restaurant have to cope with? These are all questions inhabitants might have. After doing the following analyses we can answer all these questions, and therefore optimize the placements of every function.

### Shadow Casting Analysis
The first analysis done was a shadow casting analysis. Here we calculated what voxels in our envelope  obstructed direct sunlight of neighbouring buildings. After that, the voxels that blocked too much sunlight were deleted.

First, we needed the direction of the sun for every hour throughout the whole year. This was handed to us in a CSV file by the minor coordinator. After reading the CSV file and adjusting some errors. Namely, the sun directions going underground. We named these sun directions "sunpath".

Now was the task of calculating sunlight obstruction by our voxels. The first step was building vectors with the directions of the sun. The length of each vector was 150 meters as per building codes. (see image). For every point in our envelope, we shot each vector from that point. For each vector that hit some building from the "context", Rotterdam, we added 1 to a block counter. Then we shaped the envelope by deleting each point that has more than 700 blocking vectors.

<div style="text-align: center;">
  <img src="assets/sundirection.png" alt="A single vector with the direction of the sun, representing 1 sun hour" style="width: 400px;">
  <p><em>Figure 3.2: A single vector with the direction of the sun, representing 1 sun hour</em></p>
</div>

<div style="text-align: center;">
  <img src="assets/shadowcasting.png" alt="The voxels that cast too much shadow" style="width: 350px;">
  <p><em>Figure 3.3: The voxels that cast too much shadow</em></p>
</div>

### Sunlight Analysis
Additionally, in the shadowcasting directory, we also calculated the minimum amount of direct sunlight per day for each voxel. This was done by the same principle, but reversing the direction of each vector. Since "sunpath" has hours and days encoded in it, we could count the direct sunlight per day for each voxel. We noted the voxels that did not have a minimum of 2 hours of sunlight each day and made sure that there would not be housing allocated there. But instead functions like bike parking.

<div style="text-align: center;">
  <img src="assets/notenoughsun.png" alt="The voxels that do not have enough sunlight" style="width: 350px;">
  <p><em>Figure 3.4: The voxels that do not have enough sunlight</em></p>
</div>


After that we calculated for each point the average amount of direct sunlight it would receive over the year. This was formatted to uniformly fit 1 to 0. 1 being the voxel with the biggest amount of average sun per day, and 0 being the least. This way of formatting is used for the best results when weighing points later in the process.

<div style="text-align: center;">
  <img src="assets/averagesunlight.png" alt="The average amount of sunlight" style="width: 350px;">
  <p><em>Figure 3.5: The average amount of sunlight</em></p>
</div>

### Daylight Analysis
A daylight analysis was made to calculate which voxels got the most daylight. This depends on 2 things. Firstly how much access a voxel has to the sky. And secondly how far the voxel is situated inside the building itself.

For calculating the first thing we first made a sky to have directions for vectors to shoot in. This sky was made by subdividing a sphere SOP and then converting each primitive to a point. This made sure that we got a dome of equally spaced points. This dome was placed above the envelope. Now came the calculation of how much access a voxel had to the sky. For each point, we constructed vectors from each sky point to the currently checked point. Every vector that hit another building in Rotterdam was recorded.

<div style="text-align: center;">
  <img src="assets/sky.png" alt="The sky geometry" style="width: 350px;">
  <p><em>Figure 3.6: The sky geometry</em></p>
</div>

The last part of the daylight analysis was calculating how far inside the building each voxel was. Light has to travel from the facade to each voxel, and the intensity of this light diminishes after bouncing against the building's interior. This was modelled by calculating the distance from the facade of the building to each point. This was done by first creating a shell of the envelope, using the fuse and clean SOP. Then using the distance to geometry SOP we got the shortest distance to the facade for each point. We fitted these distances in between 1 and 0. We then scaled the first daylight calculations by these fitted distances. After that we again formatted the analysis outcomes to fit uniformly in between 1 and 0, to be used by the weighing of points.

<div style="text-align: center;">
  <img src="assets/daylight.png" alt="Daylight analysis" style="width: 350px;">
  <p><em>Figure 3.7: Daylight analysis</em></p>
</div>

### Closeness Ground Analysis
The last analysis done was a calculation of how close each voxel was to the ground. This information is useful to know because for bike parking spaces you would want to be on the ground, but starter unit housing is fine being placed high up in the sky. The calculation was done by fitting the height of each point uniformly between 1 and 0, the same as we did in the previous analyses.

<div style="text-align: center;">
  <img src="assets/ground.png" alt="Closeness to the ground" style="width: 350px;">
  <p><em>Figure 3.8: Closeness to the ground</em></p>
</div>

### Noise Analysis
This analysis was not implemented but would have been very useful to us. Our recommendation is to calculate the distance between each point and sources of noise by using a "distance to geometry" SOP. These origins are the train tracks, the loud nightclubs and the busy streets. This analysis would have been used to place elderly homes further away from sources of noise. We did however minimize noise in other ways described in the forming chapter.

## Function Allocation
Now came the point of using the previous analyses in placing the functions. The first thing we did was give each point a score for each function, how much does that point want each function? Then we used a growing algorithm for placing enough voxels of each function until the area from the requirements was covered. Lastly, we quickly touch on how the parking spaces were placed.

### Weighing Points
First, we wanted for each point to create scores for each function. This was accomplished using a multi-objective optimization algorithm created by Shervin Azadi & Pirouz Nourian. This algorithm used the analysed voxel values and weighings of each function as input. The result was a list of points that contained a score for each function.

<div style="text-align: center;">
  <img src="assets/weighing.png" alt="Each point with 9 function scores" style="width: 800px;">
  <p><em>Figure 3.9: Each point with 9 function scores</em></p>
</div>

### Growing Algorithm
Now after having a score for each point, we can use a growing algorithm for placing the best possible function in each voxel. This algorithm can be, with a shortcut through the turn, described as follows:
1. Start with a single voxel seed for every function.
2. For each function:
	If there is not enough area covered already then:
		Check all the neighbouring voxels and place the next voxel at the location with the highest score.

This paragraph describes the process of creating seeds for every function. We started with a list of scored points. We then loop through all the points 9 times, once for every function that needed to be placed. For each loop, we picked the best voxel for the corresponding function, and then deleted that voxel from the total list of points, so that that voxel cannot be chosen twice. This ends us with 9 best location seeds placed at different locations in the voxel grid.

#### Neighbouring points
Now the hard part, step 2. We humans can easily divide when 2 points are neighbours, but this parametric design uses Houdini. So we first needed to define what a neighbour is which we did by constructing a grid. This was done by checking for each point the nearest 4 points in a radius of 1,9 m around that point. This radius is slightly bigger than 1,8 m to avoid floating point errors. This radius also only gets the horizontal neighbouring points, as the vertical neighbouring points are more than 3 meters away. To fetch these vertical neighbours we did not check for near points in a bigger radius, instead, we checked for points near 3,2 m above and below the current point. With this, we created a grid where every point has a maximum of 6 neighbouring points.

#### Growing each function
Take a certain function. We first calculate if there is enough area already by checking the number of points that already have that certain function. These points are called the children. Then we can check all the possible neighbours by making a set of all the neighbours of all the children. We scaled the score of the neighbours that differ in Y direction down by a small bit, this is to avoid excessive vertical growth. By then sorting that set based on the score for that certain function we pick the best one. This is the voxel that grows. Repeat for all the other functions.

<div style="text-align: center;">
  <img src="assets/growing.gif" alt="The growing process" style="width: 350px;">
  <p><em>Figure 3.10: The growing process</em></p>
</div>

<div style="text-align: center;">
  <img src="assets/outcome.png" alt="The growing outcome" style="width: 350px;">
  <p><em>Figure 3.11: The growing outcome</em></p>
</div>

### Underground Parking
After researching the underground environment of Rotterdam, we found out that we had a lot of room to dig below the surface of our site. As to not complicate the design more than it needs to we decided to place the car parking function underground. This is visualised by the blue box.

<div style="text-align: center;">
  <img src="assets/parking.png" alt="The underground parking garage" style="width: 350px;">
  <p><em>Figure 3.12: The underground parking garage</em></p>
</div>


