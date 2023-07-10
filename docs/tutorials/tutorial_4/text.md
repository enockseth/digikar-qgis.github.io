# 4. Collaborative vectorisation

The last tutorial demonstrated the vectorisation of data in a vector-based geographic data file stored locally on the computer.

The purpose of this last tutorial is to experiment with collaborative vectorisation, where each QGIS client connects to a vector data source stored on a remote geographic database.
This time, each participant will contribute to a common dataset.

# A. Preliminary steps

1. Download the data for this tutorial (~200Kb): [https://www.swisstransfer.com/d/c36a7046-2d38-4ee6-8d8e-245c37cad77b](https://www.swisstransfer.com/d/c36a7046-2d38-4ee6-8d8e-245c37cad77b)
2. Extract the zip archive to your working directory.
2. Open the QGIS project `Tutorial_4/project.QGZ`

# B. Setting-up the project

### 1. Connecting to the remote PostgreSQL database

A PostgreSQL database has been configured prior to the tutorial, and a bootstrap dataset has been uploaded.
To load this dataset as a layer in QGIS, you need first to connect to the PostgreSQL server.

1. Go to `Layer-> Data Source Manager`, and from the tab `PostgreSQL` create a `New` connection wich the following information:
- Name: give this connection a name, like "DigiKAR QGIS tutorial"
- Service: leave empty
- Host: `geohistoricaldata.org`
- Port: `5432`
- Database: `qgis-turorial`

In `Authentication`, chose `Basic` and set the following credentials:
- Username: `qgis-tutorial`
- Password: `qgis-tutorial`
Click on `store` so QGIS will store the credentials.

![](attachments/Pasted%20image%2020230709205549.png)

Click on `Test Connection` to ensure that everything is fine, and click `OK`.

2. Now, click on `Connect` to read the available geodata table from the database. The table `road_network` should be listed in the public schema. Select the table and click on `Add` to load it. ![](attachments/Pasted%20image%2020230709212538.png)

> ℹ️ Note how the layer has been loaded with a categorized symbology. This is possible because QGIS is able to store its own layer styles within the database and load them when needed.  

### 2. Setting up the map tools

Road networks are significantly more complex than the mills and calvaries points from that last tutorial, because topology comes into play.
Cependant, la topologie d'un réseau peut-être corrigée automatiquement avec un post-traitement, à condition que le réseau soit au moins navigable, c'est à dire que tous les segments d'une intersection se touchent exactement au même point.

Cette condition est impossible à respecter manuellement, mais QGIS propose des options un mécanisme de snapping qui permet de réaliser cela très facilement.
1. Go to the menu `Project->Snapping options` and enable snapping ![](attachments/Pasted%20image%2020230709214508.png).
2. Allow snapping of **vertices and segments**, leaving the default snapping distance.
3. Also toggle `Topological editing` and `Snapping on intersection`.  With topological editing, when you  move common vertices/segments, QGIS will also move them in the geometries of the neighboring feature. Snapping on intersection allows you to snap to geometry intersections of snapping enabled layers, even if there are no vertices at the intersections.
![](attachments/Pasted%20image%2020230709214809.png)

> ℹ️ More details about snapping is available in the QGIS documentation: https://docs.qgis.org/3.28/en/docs/user_manual/working_with_vector/editing_geometry_attributes.htm


# C. Let's vectorize !

**This part exercise will be carried out collectively during the session**