---
layout: default
title: "Forming"
permalink: /forming
remote_theme: pages-themes/cayman@v0.2.0
---


# Forming 
## Floorplan
The floor plan is based on the grid of the voxel format (specifically 1.8m x 1.8m x 3.2m). Using this grid ensures accessibility for everyone, including wheelchair users. Additionally, using two voxels results in a perfect dimension for the width or length of a bedroom with a double bed. Having said that, a floor plan for the 3rd floor was generated through region growing with functions. When arranging the 3rd floor, consideration was given to the presence of functions and the approximate size of these regions.
Subsequently, the pattern of the voxel formation was adopted and redrawn in Revit. In creating the floor plans, particular attention was paid to how all the functions come together in harmony. For the 3rd floor, the focus was on elderly units, shared housing, laundry service, and the gym. In Houdini, colors were used to illustrate how the functions are interconnected.

*screenshot Houdini functies plaatsing

The floor plans were initially drawn for the elderly units, followed by one for the shared unit, and then the gym and laundry service. On the west side of this floor, a significant portion is dedicated to elderly units. This is an area where shared spaces are provided for the elderly to play games or chat with each other. An additional outdoor space has been created in this wing to give the elderly their own place.

*fot gezamenlijke ruimte

In the middle of the floor plan, there is also a large outdoor space for the entire residential complex. Residents of this building can enjoy the beautiful garden, providing more companionship for the elderly when they go outside. On the east wing, there are additional elderly units and a shared unit. While there was an option to make this east wing exclusively for elderly units, mixing in a shared unit promotes more social interactions without being too conspicuous. Further consideration was given to adding a laundry room and a gym deeper into the east wing. This laundry room can be used by various residents of the building, specifically for those on this floor.

<div style="display: flex; justify-content: center;">
  <div style="text-align: center; margin-right: 50px;">
    <img src="assets/sfeer1.jpg" alt="Sfeer1" style="height: 250px;">
    <p style="margin-top: 10px;"><em>Figure 4.3: Sfeer1</em></p>
  </div>
  <div style="text-align: center;">
    <img src="assets/sfeer2.jpg" alt="Sfeer2" style="height: 250px;">
    <p style="margin-top: 10px;"><em>Figure 4.4: Sfeer2</em></p>
  </div>
</div>


Lastly, we were unable to conduct an analysis to determine the optimal location for a shaft, elevators, and stairwell. However, we managed to place them in advantageous positions using our knowledge of construction and architecture. In the east wing, a central stairwell with two spiral staircases has been positioned, serving as an additional secure escape route. The same applies to the west facade, where three elevators have been installed due to the larger surface area compared to the east wing. At the entrance of the west wing, two elevators with shafts have been installed, accompanied by two spiral staircases. Furthermore, an additional elevator has been added deeper into the west wing, primarily because the distance to the centre of the wing is considerable from the furthest point. This would make it challenging to meet the desired requirements of the building code for evacuation. The inclusion of an extra elevator, shaft, and stairwell with a spiral staircase significantly enhances the safety of evacuation from the surrounding residences in this wing.
![Plattegrond](plattegrond.png)
## Facades

It aimed to create an interesting and appealing structure that radiates unity and creates a comfortable environment. 
This all needed to be included in the design, without losing  its functionality.

Because of the location, the design faces some challenges, for example, noise from the trains and Biergarten, and optimizing sunlight. 
Also, the design process accounted for factors such as privacy and the maximizing use of greenery. This way, habitants and visitors would feel safe and at ease in the building.

### Tile set

In Figures 4.1 and 4.2, the full facade tile set is shown. It consists of 8 tiles. However, in the figure, 9 tiles are shown. the first two shown tiles are the same tile but mirrored. 

<div align="center">
  <img src="assets/Facadetileset.jpeg" alt="Tiles set" style="width: 400px;">
  <p><em>Figure 4.1: Tiles set</em></p>
</div>
<div align="center">
  <img src="assets/tilesAfterRender.jpg" alt="Tiles set after render" style="width: 400px;">
  <p><em>Figure 4.2: Tiles set after rendering</em></p>
</div>

For the lower floors, the facades had to be welcoming and open. The design of the facades on these floors uses big, rectangular windows with a triangular cut-out. This is the third tile shown in Figures 4.1 and 4.2. This creates a triangular wave across the floor as shown in Figure 4.3.

<div align="center">
  <img src="assets/lowerFacades.jpeg" alt="lower facades" style="width: 250px;">
  <p><em>Figure 4.3: Facades with trianglular wave</em></p>
</div>

The floors above are mostly expected to be housing. For these floors sunlight and noise reduction are more important. Also in the spirit of creating unity across the building, the triangular theme was re-introduced in these types of facades. For the North and East sides of the building, the majority of the noise pollution is expected to be from directly below the facades from the trains or Biergarten. For the other two sides of the building, the majority of the noise pollution will come sideways from these noise sources. Therefore, two similar but different kinds of facades were created. The facades on the North and East sides will form horizontal lines and a hanging shape (tiles 5 and 6 in Figures 4.1 and 4.2), and the facades on the South and West sides will have quite similar shapes, however, these will be vertical/standing shapes (tiles 7 and 8 in Figures 4.1 and 4.2).
These tiles all have a non-window side. These will be aimed at the main source of the noise and will have a sound-reducing material. The window will allow more noise into the room/building. Therefore this side of the facades will face the other way. Using the concept "if you can't see it, you can't hear it" allows less noise to enter the rooms through the windows. Although this creates lessened noise pollution, it has to be noted this won't fully resolve the noise problem per definition. Using plants for the top of the balcony railings will help improve temperature control and it creates a less industrial or concrete construction look. The greenery on the balcony might even help a little bit with noise reduction but this will be minimal.

In Figures 4.1 and 4.2, there are two more tiles shown, the 4th and the last tile. The 4th tile is used as an emergency exit and the last tile is a big window, used at inward corners where the usual facades can't be used due to lack of space.

For the area, it's important to have nice green areas. The little amount of greenery that will be lost in the construction of the building has to be made up for. Better would be better if more greenery were placed than initially was placed at the location. Therefore, it was chosen to use green roofs. The tile corresponding to the roof is shown on the right in Figure 4.4. In this figure, also the floor tile is shown, left of the roof tile.

<div align="center">
  <img src="assets/FloorAndRooftile.png" alt="lower facades" style="width: 300px;">
  <p><em>Figure 4.4: Floor and roof tiles</em></p>
</div>

## Placing the correct tiles using houdini
This subsection describes how we placed the abovementioned tileset on the voxelcloud using Houdini. To parametrically make such a design is quite a difficult task. But also quite rewarding, since this last step finally gives you a visual representation of the building we have been designing so far. Placing these tiles went as follows.

Since the tiles should have the same dimensions as the voxels, we started by building a voxel only building by using a copy to points and fuse SOP. This allows us to have access to different orientated walls, by selecting primitives with correctly orientated normals. We created 4 groups: floors, ceilings, roofs and facades.

The floors were placed by copying the floor tiles onto the primitives and rotating them according to the voxel rotation. The ceilings were done in a similar manner except their normal direction was reversed. The roof tiles have been placed by manually selecting the upper most floors. This was not done parametrically and is thus a form of post processing. This was because of the design decision to include a walkable area in the hole of the building.

![Floor, ceiling and roof](assets/Vloerplafonddak.png)

Floor, ceiling and roof.

The facades facing north and east were selected by their normals, and this selection was reversed to get the facades facing south and west. Placing balconies was done by placing one every 5 tiles. The lowest 4 floors were each put in a different group based on their height. Then each floor got a different tile to use. The emergency exits were placed manually.

![Finished Building](assets/GeheleGebouw.png)

Finished Building.

### Corners
If you look closely you might spot a problem with these tile placements. That is, tiles placed on inward corners clash with eachother (see image). An attempted solution was for each occupied voxel keeping track of neighbouring voxels, to determine a neighbour count. This however was not enough to determine if an inward corner has been placed, only for outward corners. A better solution would have been to for each unoccupied voxel, determine if they have 2 or more occupied neighbours. If so, replace the tiles with the inward corner tile.

![Clashing Corners](assets/botsendeTiles.png)

Clashing Corners.




