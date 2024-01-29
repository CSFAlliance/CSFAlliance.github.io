layout: default
title: "Massing"
permalink: /massing
remote_theme: pages-themes/cayman@v0.2.0
---

# Massing
This chapter describes how we translated concepts and diagrams into a parametric building program using Houdini. We first start by telling how the site and envelope were created. Then how different analyses were done to improve the envelope. The last part of the massing chapter talks about allocating functions to their analysed locations. The following flowchart can be used as reference for the algoritm in Houdini.
(afbeelding process flowchart)

## Voxel Cloud
First we needed the dimensions and outline of the site we were building on. Firstly we had to import the neighbouring building of Rotterdam. This was realised using importing tiles from the bag3d website. Now we carefully traced the outline of the site by using the neighbouring buildings. The maximal building height the municipality of Rotterdam stated for our area was 200 meters! So we did not limit ourself, and made our envelope 200 meters high.
(afbeelding envelope)

To be able to reason about this envelope in detail, we need to subdivide the envelope in voxels. But what sizes to choose? First we thought about making the voxels 1,5 x 4 x 1,5 m. It was important for us to adhere to the standards of Neufert, therefore we made the horizontal dimensions multiple of 0,3 m. This is very useful for designing modular spaces of 0,3 m. 1,5 m was a good minimum we chose, since the smallest possible function was a hallway. That hallway had to be at least 1,5 m wide to accomodate turning wheelchairs. However, we did not account for the structal requirements of each voxel, since there needs to be a wall and or other support beams in each voxel. Therefore we raised the horizontal dimension of the voxel to 1,8 x 1,8 m.
(afbeelding neufert modularity)

The height of 4 m was chosen using a wet thumb and looking at the highrise building EWI. But after realising 4 m was way too high we settled for 3,2 m: 2,5 m headspace and 70 cm for the floors and ventilation systems.

### Underground Parking
After researching the underground environment of Rotterdam, we found out that we had a lot of room to dig below the surface in our site. We decided to place the car parking garage underground. This is visualised by the blue box.
(afbeelding underground parking)

## Analyses
Now that we have an envelope, we want to inhabit this envelope with different functions. But how to decide where to place each function? This is done by thoroughly analysing the current envelope. Is there enough sunlight in my house? How much noise does my restaurant have to cope with? These are all questions inhabitants might have. After doing the following analyses we can answer all these questions, and therefore optimize the placements of every function.

### Shadow Casting Analysis
The first analysis done was a shadow casting analysis. Here we calculated what voxels in our envelope  obstructed direct sunlight of neighbouring buildings. After that, the voxels that blocked too much sunlight were deleted.

First we needed the direction of the sun for every hour thoughout the whole year. This was handed to us in a csv file by the minor coordinator. After reading the CSV file and adjusting some error. Namely the sun directions going underground. We named these sun directions "sunpath".

Now was the task of calculating sunlight obstruction by our voxels. First step was building vectors with the directions of the sun. The length of each vector was 150 meters as per building codes. (zie afbeelding vector constructie). For every point in our envelope we shot each vector from that point. For each vector that hit some building from the "context", Rotterdam, we added 1 to a block counter. Then we shaped the envelope by deleting each point that has more than 700 blocking vectors.
(afbeelding uitkomsten schaduw analyse)

### Sunlight Analysis
Additionally, in the shadowcasting directory, we also calculated the minimum amount of direct sunlight per day each voxel got. This was done by the same principle, but reversing the direction of each vector. Since "sunpath" has hours and days encoded in it, we could count the direct sunlight per day for each voxel. We noted the voxels that did not have a minimum of 2 hours of sunlight each day, and made sure that there would not be housing allocated there. But instead functions like bike parking.

After that we calculated for each point the average amount of direct sunlight it would recieve over the year. This was formatted to uniformly fit 1 to 0. 1 being the voxel with the biggest amount of average sun per day, and 0 being the least. This way of formatting is used for the best results when weighing points later in the process.
(afbeelding uitkomsten zonlicht analyse.)

### Daylight Analysis
A daylight analysis was made to calculate which voxels got the most daylight. This depends on 2 things. Firstly how much access a voxel has to the sky. And secondly how far the voxel is situated inside the building itself.

For calculating the first thing we first made a sky to have directions for vectors to shoot in. This sky was made by subdividing a sphere SOP, and then converting each primitive to a point. This made sure that we got a dome of equally spaced points. This dome was placed above the envelope. Now came the calculation of how much access a voxel had to the sky. For each point we consctructed vectors from each sky point to the currently checked point. Every vector that hit another building in rotterdam was recorded.

The last part of the daylight analysis was calculating how far inside the building each voxel was. Light has to travel from the facade to each voxel, and the intensity of this light diminishes after bouncing against the building interior. This was modelled by calculating the distance from the facade of the building to each point. This was done by firstly creating a shell of the envelope, using the fuse and clean SOP. Then using the distance to geometry SOP we got a shortest distance to the facade for each point. We fitted these distances in between 1 and 0. We then scaled the first daylight calculations by these fitted distances. After that we again formatted the analysis outcomes to fit uniformly in between 1 and 0, to be used by the weighing of points.
(afbeelding uitkomsten daglicht analyse.)

### Closeness Ground Analysis
The last analysis done was a calculation of how close each voxel was to the ground. This information is useful to know because bike parking spaces you would want to be on the ground, but starter unit housing is fine being placed high up in the sky. The calculation was done by fitting the height of each point uniformly in between 1 and 0, the same as we did the previous analyses.
(afbeelding closeness ground analysis)

### Noise Analysis
This analysis was not implemeted, but would have been very useful to us. Our recommendation is to calculate the distance between each point and sources of noise by using a "distance to geometry" SOP. These origins are the train tracks, the loud nightclubs and the busy streets. This analysis would have been used to place elderly homes further away from sources of noise. We did however minimize noise in other ways described in the forming chapter.

## Function Allocation
Now came the point of using the previous analyses in placing the functions. The first thing we did was giving each point a score for each function, how much does that point want each function. Then we used a growing algorithm for placing enough voxels of each function until the area from the requirements was covered. 

### Weighing Points
First we wanted for each point to create scores for each function. This was accomplished using a multi objective optimization algorithm created by Shervin Azadi & Pirouz Nourian. This algorithm used the analysed voxel values and weighings of each function as input. The result a list of points that contained a score for each function.
(afbeelding geometry spreadsheet)

### Growing Algorithm
Now after having a score for each point, we can use a growing algorithm for placing the best possible function in each voxel. This algorithm can be, with a shortcut trough the turn, described as follows:
1. Start with a single voxel seed for every function.
2. For each function:
	If there is not enough area covered already then:
		Check all the neighbouring voxel and place the next voxel at the location with the highest score.

This paragraph describes the process of creating seeds for every function. We started with a list of scored points. We then loop trough all the points 9 times, once for every function that needed to be placed. Each loop we picked the best voxel for the corresponding function, and then deleted that voxel from the total list of points, so that that voxel cannot be chosen twice. This ends us with 9 best location seeds placed at different locations in the voxel grid.

#### Neighbouring points
Now the hard part, step 2. We humans can easily devide when 2 points are neighbours, but this parametric design uses Houdini. So we first needed to define what a neighbour is which we did by constructing a grid. This was done by checking for each point the nearest 4 points in a radius of 1,9 m around that point. This radius is slightly bigger than 1,8 m to avoid floating point errors. This radius also only gets the horizontal neighbouring points, as the vertical neighbouring points are more than 3 meter away. To fetch these vertical neighbours we did not check for near points in a bigger radius, instead we checked for points near 3,2 m above and below the current point. With this we created a grid where every point has at maximum 6 neighbouring points.

#### Growing each function
Take a certain function. We first calculate if there is enough area already by checking the amount of points that already have that certain funcion. These points are called the children. Then we can check all the possible neighbours by making a set of all the neighbours of all the children. By then sorting that set based on the score for that certain function we pick the best one. This is the voxel that grows. Repeat for all the other functions.
(afbeelding gif groeien)
