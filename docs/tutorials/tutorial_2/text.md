# 2. Georefencing historical maps
# A. Preliminary steps
1. Download the data for this tutorial (⚠️~1.6Gb): https://www.swisstransfer.com/d/d9b7b3c8-8c4a-4b7d-b988-acd20da73d69
2. Extract the zip archive to your working directory.
3. Open the preconfigured QGIS project  `Tutorial_2/project.QGZ`
4. Select a *MeilenBlatt* to georeference in the list available in the layer `georeferencing_index`. **Each participant should chose a different sheet**.
5. In QGIS, enable automatic snapping from `Project->Snapping options...`. Toggle ![[Pasted image 20230705183316.png]] and keep the default options:
![test](attachments/Pasted image 20230705183425.png)
This will make the cursor automatically *snap* to the closest vertex in the vector layers during coordinate capture.

# B. Fast interactive georeferencing

> 🛠️

1.  Install the "Freehand raster georeferencer" plugin. After installing the plugin, a set of tools will appear in the QGIS toolbar: 
![](attachments/Pasted%20image%2020230705172819.png)
2. Click on ![](attachments/Pasted%20image%2020230705172837.png) and open the *Meilenblatt* that you have chosen. Use the tools `MO[ve]`, `RO[tate]`, `SC[ale]` and `2P` (2 points transform) to roughly fit the sheet in its corresponding cell in the grid. You can also try to approximately match the places in the sheet with the point places in the vector layer `hov_places`.
![](attachments/Pasted%20image%2020230705173600.png)
3. Once you are satisfied with the result, click on ![](attachments/Pasted%20image%2020230705173726.png) and save it to a file named `d_{sheet_number}_georeferenced_fast.png` in the folder `Tutorial_2/output`.
4. Open the `Tutorial_2/output` directory using your computer's file browser. You will notice that the georeferenced file is accompanied by a `.pgw` file. This simple text file is known as a [_World File_](https://fr.wikipedia.org/wiki/World_file), which contains the parameters for the rigid geometric transformation used by the GIS to display the PNG image within a specific coordinate reference system. You can open this file with a text editor and refer to the description of the [WorldFile format on Wikipedia](https://de.wikipedia.org/wiki/World-File) to see what information is required to display a georeferenced image."

> ℹ️ When copying/moving a georeferenced image that has a companion World File, make sure to also copy or move the world file along with it!

> ℹ️Here, the georeferenced PNG has not been distorted during the georeferencing process. The output PNG is identical to the original image, and all the georeferencing information is stored in the World File. This is possible because the Freehand georeferencer only performs linear transforms. However, with more advanced georeferencing techniques, the original image often needs to be locally distorted. In such cases, the resulting georeferenced image differs from the original one.

# C. Georeferencing with ground control points: the grid technique

## a. Introduction
The freehand raster georeferencer is an incredibly valuable tool for quick and approximate georeferencing. However, its effectiveness is often limited when precise georeferencing is required.

Historical maps typically have poor planimetric accuracy. Aligning them with a reference map, such as OpenStreetMap, often involves distorting the historical maps to ensure their content aligns with the reference. This distortion is necessary to achieve a proper match between the two maps.

Linear transformations are generally not sufficient for historical maps. Non-linear methods such as **polynomial transformations** or **thin plate splines** need to be employed to achieve more accurate and precise results.

Such methods require a set of **ground control points (GCP)**. The idea behind GCPs is to establish a correspondence between a pixel position in the source image and a geographic or projected position in an coordinate reference system.

Thus, a GCP is a pair of points that are typically placed on the depiction of the same geographic entity both in the image and on a reference map already in a geo/projected CRS like, for instance, the OpenStreetMap basemap.

These entities can be anything, depending on the image to process: buildings, natural objects, roads, ...

Unfortunately, the landscapes depicted in historical maps often no longer exist, making it challenging to find suitable GCPs.

Furthermore, when dealing with maps composed of multiple sheets, such as the _Meilenblätter_, placing GCPs solely on geographic entities does not provide control over how adjacent sheets should be stitched together. In other words, the original grid structure of the sheet is not preserved during the georeferencing process.

In soùme cases, maps like the Meilenblätter include a cartographic grid with well-documented parameters such as its resolution, dimensions, and orientation, which allows for its reconstruction in the geographic reference system.

A common and effective approach is to define GCPs on this reconstructed grid. This approach ensures that the georeferenced sheets align correctly with each other. It also helps prevent interpretation errors regarding the correspondence between GCPs placed on different geographic entities. This method provides a robust solution while maintaining accuracy during the georeferencing process.

## b. GCP-based georeferencing on the *Meilenblätter* grid

> 🛠️

1. Open the QGIS georeferencer from the menu `Layer->Georeferencer`.
2. Click on ![](attachments/Pasted%20image%2020230705182811.png) to load the the original, non-georeferenced *Meilenblatt* .
3. Use the `Add Point` tool ![](attachments/Pasted%20image%2020230705183013.png) to define GCPs on each corner of the sheet, both in the image view and the map view, using the recreated grid `georeferencing_index`.
![[Pasted image 20230705184245.png]]
4. Save the GCPs in  `Tutorial_2/output/d_{sheet_number}.png.points` using ![](attachments/Pasted%20image%2020230705184956.png) (or from `File->Save GCP Points as...`)
5. Next, click on `Transformation Settings` ![](attachments/Pasted%20image%2020230705184409.png) to access the settings for the transformation. Here, you can specify the type of transformation method to be applied and the target Coordinate Reference System (CRS), among other options.
To ensure the corners of the image match precisely, select "Thin Plate Spline" as the transform type and set the target CRS to `EPSG:3398 (RD/83 / 3-degree Gauss-Kruger zone 4)`.

6. Save the georeferenced image as `Tutorial_2/output/d_{sheet_number}_georeferenced_grid.tif` and keep the resampling method unchanged:
![](attachments/Pasted%20image%2020230705184913.png)

7. Click `OK` to confirm the settings and initiate the georeferencing process by clicking the button !![](attachments/Pasted%20image%2020230705185320.png). The georeferenced sheet should then be automatically loaded into the project.

8. Click `OK` and run the georefencing with the button![](attachments/Pasted%20image%2020230705185320.png). The georeferenced sheet should be automatically loaded into the project.
9. Notice how the georeferenced sheet does not align well with the HOV places or the OSM basemap. This is the result of two factors: the imperfect accuracy of the grid and the inherent spatial inaccuracies within the *Meilenblätter*. To achieve a more accurate georeferencing of the map content, we need to locally warp it. This can be accomplished by adding GCPs directly onto the geographic entities depicted in the map.

# D. Georeferencing with ground control points: GCPs on persistent entities

Because setting GCPs only on entities would cause mismatches between adjacent sheets, we want to retain the GPCs put on the grid corners increase the number of control points.

This time we will use the OSM basemap as the reference.

> 🛠️

1. Open the georeferencer and load the original sheet image once again. Import the grid GCPs that were previously defined using the button ![](attachments/Pasted%20image%2020230705190605.png). 
2. Look for entities that are depicted both in the OSM reference map and in the sheet, i.e. entities in the _Meilenblatt_ that still exist today. The best candidates for GCPs are **churches** since they were commonly used as reference points during the topographic survey of the _Meilenblätter_.

> ℹ️ There is no golden rule on the placement strategy for GCPs as it varies depending on the specific map being processed. However, there are some best practices that can help achieve higher-quality georeferencing:
- The more GCPs, the better, but it's preferable to have a few reliable points rather than many questionable ones.
- Aim to cover the entire sheet with GCPs. Since the geometric transformation relies solely on the information provided by the GCPs, it's best to avoid leaving large areas of the sheet empty of GCPs to prevent dramatic distortions in those areas.
- Tall and compact structures such as churches, bell towers, and castles are excellent candidates for GCPs. They were often used as triangulation points during the topographic survey of historical maps, making their positions on the map more accurate and reliable compared to features like roads or mountain summits.
![](attachments/Pasted%20image%2020230705191553.png)
![](attachments/Pasted%20image%2020230705191606.png)
<small>Figure: The same church depicted in a Meilenblatt and on OSM. The point is the location of that place regirstered in the HOV.</small>

3. Once you have added a reasonable number of control points, run a new georefenrencing using the same transformation as before. Save the georeferenced image as `Tutorial_2/output/d_{sheet_number}_georeferenced_dense.tif`.

ℹ️ Feel free to run intermediate georeferencings. Working incrementally and adding/adjusting GCPs along the way helps maintain control over their quality and improves the final outcome.

4. Finally, save the GCPs as `Tutorial_2/output/d_{sheet_number}_dense_.png.points`.
5. Compare the three different georeferencings visually and observe the effects of the warping on the resulting map, particularly on the edges of the sheet.

# E. Clipping the georeferenced raster

You've certainly noticed that the sheet is framed by a black border that contains no cartographic information and that overlaps with adjacent sheets

![](attachments/Pasted%20image%2020230709140813.png)

It would be better to remove this border and clip the georeferenced raster so that it fits into its corresponding grid tile.

The vector grids `grid_sheets` and `georeferencing_index` have been designed so that each tile is a polygon. We're going to reuse the tile polygone as a mask shape to clip the georeferenced sheet.

1. 1. First, we need to isolate the tile corresponding to the sheet into a separate layer. Hide the georeferenced sheet  by unchecking it in the Layer Pane si the the tile in `georeferencing_index` becomes visible. Select this layer in the Layer Pane, then with the map tool `entity selection` ![](attachments/Pasted%20image%2020230709141322.png)  click on the tile to select it. 
![](attachments/Pasted%20image%2020230709144216.png)
3. Hit `CRTL+C` or go to `Edit->Copy entities` to copy the selected tile polygon.
4. Then,  go to  `Edit->Paste entitie as...-> New Vector Layer` to paste the polygon to a new layer and save it  as a GeoJSON  file to `Tutorial 2/output/mask.geosjon`. The resulting layer, containing only the copy-pasted tile, should be automatically loaded to QGIS as a new layer named `mask`.
5. Now go to the menu `Raster->Extraction->Clip vector layer by mask layer` and configure clipping as below. Don't forget to check "Create an output alpha band" so that clipped borders are transparent.
![](attachments/Pasted%20image%2020230709142426.png)
5. This will create a new georeferenced TIF file. Choose to save it as `Tutorial 2/output/d_{sheet_number}_georeferenced_clipped.tif`, and click `Run`.
6. The final map, georeferenced and without black borders, should now be displayed in QGIS:
![](attachments/Pasted%20image%2020230709142949.png)



<center>This ends the tutorial on georeferencing in QGIS !</center>

Please do no delete the files `d_{sheet_number}_georeferenced_clipped.tif` and `d_{sheet_number}_georeferenced_clipped.tif`  . 
We'll collect them at the end of the session to create a single georeferenced map based on everyone's contributions.
Thank you !