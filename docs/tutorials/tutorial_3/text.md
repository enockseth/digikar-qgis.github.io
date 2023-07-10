# 3. Introduction to vectorisation in QGIS

This short tutorial is a  introduction to the basics of vectorisation in QGIS with very simple data. The goal is to manually capture the mills and calvaries that are pictured on the Meilenblätter with dedicated symbols:

| Symbol      | Description |
| ----------- | ----------- |
| ![](attachments/Pasted%20image%2020230709195736.png) ![](attachments/Pasted%20image%2020230709200205.png)     | Mühle: symbol of a waterwheel or a windmill       |
| ![](attachments/Pasted%20image%2020230709200416.png)   | Kreuzweg: black cross often at crossroads.        |


# A. Preliminary steps

1. Download the data for this tutorial (~200Kb): https://www.swisstransfer.com/d/24a42765-42ac-49a1-83f2-5c8b6cf04e82
2. Extract the zip archive to your working directory.
1. If needed, copy the folder `Tutorial_3` to your working directory.
2. Open the QGIS project `Tutorial_3/project.QGZ`

# B. Creation of a new vector layer

1. Create a new point layer from `Layer->Create Layer->New Temporary Scratch Layer`. Create a layer named "Point places" composed of point entities whose coordinates are in the CRS EPSG:3398. This layer should also include a field for categorizing the point place. Follow the instructions in the figure below:
![](attachments/Pasted%20image%2020230709191223.png)
3. At the moment, the layer is only temporary and stored in RAM, as shown by the chipset icon next to the layer name: ![](attachments/Pasted%20image%2020230709191715.png). Save this layer as a GEOJSON file in the `Tutorial_3/output` folder under the name `meilenblaetter_pointplaces.geojson`. Once saved, the file will be automatically opened by QGIS. You can safely delete the temporary layer (`right click->Remove Layer`).

# C. Getting started with vectorisation

Dans QGIS, toutes les couches sont par défaut en lecture seule.Pour que leur modification soit possible, il faut d'abord toggle le mode édition.


1. Activate edit mode for the `Point places` layer: Select it in the Layer Pane, then `Right click->Toggle editing` or click on the ![](attachments/Pasted%20image%2020230709192801.png) icon in the QGIS toolbar. A small pencil icon in front of the layer name in the Layer Pane indicates that the layer is in "write mode": ![](attachments/Pasted%20image%2020230709192901.png)
2. In the QGIS toolbar, activate the map tool `Add Point Feature` and add a point anywhere on the map (don't worry, this is just a test!). This should open a pop-up form to fill in the attributes of the point feature just created. You can see that it's a free text field, defaulting to NULL. Not a very satisfying form! Let's see how we can improve it. Click `Cancel` to cancel entity creation. 
![](attachments/Pasted%20image%2020230709193223.png). 
4. Open the `Point places` layer properties (`Right click->Properties`) and look for the `Attribute Form` tab.  We're going to configure the input form ourselves. Change the form type value from `Autogenerate` to `Drag and Drop Designer`. This opens an editor that lets you compose an form by simply dragging and dropping from the left-hand column `Available Widgets` to the right-hand column `Form Layout`.
5. The field `placetype` should be in the `Form Layout` column by default. Select the field to configure it in detail, so that:
	- Its display alias should be "Place type (according to the map)".
	- The widget is a combo box (i.e. Value Map) with two possible values:

| Value      | Description |
| ----------- | ----------- |
| Mühle      | Mühle: symbol of a waterwheel or a windmill      |
| Kreuzweg   | Kreuzweg: black cross often at crossroads.        |

![](attachments/Pasted%20image%2020230709202533.png)

If you try to create a point again, the form is updated with your settings:

![](attachments/Pasted%20image%2020230709194944.png)

5. Save the project ![](attachments/Pasted%20image%2020230709200831.png) . The custom form will be stored in the QGZ file.

6. Try to find at least one mill and one calvary inside the `Case study areas` ! 

> ℹ️ Mills can be found in the "Schönburgische herrschaften" and  "Bautzener Kreis" region. Calvaries/"Kreuzwegs" are scattered across the "Bautzener Kreis" region.

7. Regularly save your edits, either by:
	- untoggling the `edit mode` ![](attachments/Pasted%20image%2020230709200940.png) to force saving
	- or clicking on the ![](attachments/Pasted%20image%2020230709201009.png) button just to the right of ![](attachments/Pasted%20image%2020230709200940.png), or right-clicking on the layer in the Layer Pane and selecting `Save layer edits`.


<center>This ends the first trials with vectorisation in QGIS !</center>

Please do no delete the file `meilenblaetter_pointplaces.geojson` and, at the end of the day, send the file to `bertrand [at] dumenieu [dot] ehess [dot] fr`. They will contribute to the DigiKAR datasets. Thank you !