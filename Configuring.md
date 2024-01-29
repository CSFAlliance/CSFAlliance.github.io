---
layout: default
title: "Configuring"
permalink: /configuring
remote_theme: pages-themes/cayman@v0.2.0
---

# Configuring 
# Process

## REL-chart

With the created set of program requirements, we created a diagram to specify the preferences and the preferred spatial qualities.

Each function will need to be linked to another function. Some are strongly coupled, while others are less strongly coupled. To gain a comprehensive overview, we have listed the functions. Each function is assessed in relation to other functions through weights ranging from 0 (lowest weight) to 1 (highest weight).

<div align="center">
  <p><em>Table 2.1: REL-chart</em></p>
  <img src="assets/RelChart.png" alt="RelChart" style="width: 700px;">
</div>

For the parking spaces, bike spaces, and the restaurant, it was crucial that these be located close to the street and the beer garden. These functions must remain easily accessible for both the building residents and visitors. 
Furthermore, it is paramount that the elderly units are situated near the care centre for the elderly and the gym. Seniors require physical activity and should have access to assistance within a short distance. Conversely, it is beneficial for physiotherapists and nurses to be close to the homes of the elderly to provide optimal support. 
For all units in the building, it is also important that they can easily reach the laundry rooms. Often, there may be an elevator or corridor involved before reaching the designated space. The only consideration is to avoid mixing elderly units with various other residential units. Older individuals generally prefer a more tranquil environment in their living spaces. Nevertheless, integrating the elderly with the youth and other demographics can create a sense of community and prevent them from feeling isolated. 
Finally, it seemed like a good idea to have a courtyard on the ground level. A communal space where different people can come together, including residents of the building, visitors, and individuals from the surrounding area. 
Then we also created another rel-chart of what features should meet based on various analyses.

<div align="center">
  <p><em>Table 2.2: REL-chart extension</em></p>
  <img src="assets/Relchart2.png" alt="Relchart2" style="width: 350px;">
</div>

For all inhabited spaces (residences, care center, gym, and restaurant), we have stipulated that sunlight should be able to enter, and the same applies to the sky view factor. It is noteworthy that the elderly units have high weights (specifically 1) for these aspects because it has been proven that seniors often recover more quickly when there is something to observe, and they can see/hear nature. It is important to pay special attention to the view and sunlight for the elderly. While preferences for the other units also exist, they are comparatively less critical. 

Additionally, it is crucial that other functions outside the building, such as the Beer Garden and the courtyard, receive an adequate amount of sunlight. The building should not obstruct light for these existing structures. 

Furthermore, it is important, especially for residential units, to minimize the impact of loud noises from the surroundings. The project location is adjacent to the railway, so this factor needs to be taken into account. 

For certain functions, it is also vital that they are situated close to the ground floor. Initially, we determined that communal spaces should primarily be close to the ground level. The higher the building, the more private it becomes. Additionally, it was important for parking spaces to be underground, so we assigned a value to them in relation to all other functions. 

## Flowchart 

To facilitate the process as effectively as possible, we have maintained a flowchart outlining the progression of the process leading to the creation of the building. Various components need to be investigated. Through the use of the flowchart, it becomes clearer for us to determine the approach and make progress in the project. 

<div align="center">
  <img src="assets/Flowchart.jpg" alt="Flowchart" style="width: 900px;">
  <p><em>Figure 2.1: Flowchart</em></p>
</div>

## Voxel Cloud
The first steps for creating a parametric design in Houdini involved creating a voxel cloud. We needed the dimensions and outline of the site we were building on. Firstly we had to import the neighbouring building of Rotterdam. This was realised using importing tiles from the bag3d website. Now we carefully traced the outline of the site by using the neighbouring buildings. The maximal building height the municipality of Rotterdam stated for our area was 200 meters! So we did not limit ourself, and made our envelope 200 meters high.
(afbeelding envelope)

To be able to reason about this envelope in detail, we need to subdivide the envelope in voxels. These voxels indicate the internal volumes of the generated building. The voxel size has to be chosen wisely to prevent creating awkward or unusable spaces. The height of a voxel is based on the height of one floor of the building. Because of "Bouwbesluit", the ceiling of newbuild housing space has to lie at least 2.6 meters high.

First we thought about making the voxels 1,5 x 4 x 1,5 m (WxLxH). It was important for us to adhere to the standards of Neufert, therefore we made the horizontal dimensions multiple of 0,3 m. This is very useful for designing modular spaces of 0,3 m. 1,5 m was a good minimum we chose, since the smallest possible function was a hallway. This area is based on the minimum area needed for a wheelchair to turn ([Figure 2.2](#wheelchair-turn)) and the standard wingspan of a person ([Figure 2.3](#Person-size)). However, we did not account for the structal requirements of each voxel, since there needs to be a wall and or other support beams in each voxel. Therefore we raised the horizontal dimensions of the voxel to 1,8 x 1,8 m.

The height of 4 m was chosen using a wet thumb and looking at the highrise building EWI. But after realising 4 m was way too high we settled for 3,2 m: 2,5 m headspace and 70 cm for the floors and ventilation systems. This resulted in a voxel size of 1.8x1.8x3.2 (WxLxH) m^3.
  

<div style="display: flex; justify-content: center;">
  <div style="text-align: center; margin-right: 10px; max-width: 50%;">
    <img src="https://github.com/CSFAlliance/csfalliance.github.io/assets/93916885/423155a7-9e08-46ef-bafa-203f8e75ff69" alt="Wheelchair 180 degrees turn with one wheel at a fixed point" style="height: 300px;">
    <p style="margin-top: 10px;"><em>Figure 2.2: Wheelchair 180 degrees turn with one wheel at a fixed point</em></p>
  </div>
  <div style="text-align: center; max-width: 50%;">
    <img src="https://github.com/CSFAlliance/csfalliance.github.io/assets/93916885/c659e5df-4efd-4539-b298-9f37dfe98c6a" alt="Person height and arm span" style="height: 300px;">
    <p style="margin-top: 10px;"><em>Figure 2.3: Person height and arm span</em></p>
  </div>
</div>

source: prof. ir. Haak, A.J.H. & ir. Leever-van der Burgh, D., "De Menselijke Maat", Delftse universitaire pers (1980)






