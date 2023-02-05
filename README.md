# MicroChargeNet
This Repo shows an approach to create equidistant Positions for Micromobility Charging Points by using the Eclipse SUMO Simulation Environment. 

## Load SUMO-Net and create the mesh
Edit the *basePath* to the directory of the Simulation:
```
basePath = 'PATH/TO/SIMULATION'
```
Edit the *estimatedX* and *estimatedY* varibles to the size of the map in meter:
```
estimatedX = size_in_meter
estimatedY = size_in_meter
```
Edit the *distance* between the points in the mesh:
```
distance = distance_in_meter
```
<img width="600" alt="CalcMesh" src="https://user-images.githubusercontent.com/38497435/216817124-b9fc3f9f-1a6c-4284-b020-8bc37f105599.png">

## Calc exact points for the charging points:
For every point in the mesh find the next lane for a charging station
```
lanes = net.getNeighboringLanes(point[0], point[1], radius, includeJunctions=False)
```
With the parameter *includeJunction* you can specify whether junctions can also be calculated for a station. 

The charging point is be placed in the middle of a lane.
```
xy = sumolib.geomhelper.positionAtShapeOffset(nearestLane[0].getShape(), nearestLane[0].getLength()/2)
```

<img width="600" alt="MeshAndPoints" src="https://user-images.githubusercontent.com/38497435/216817593-3df7a7ae-3616-47c5-97c3-bcd3c46ce2cd.png">
Red points are the calculated mesh and the green ones are the charging stations

## Filter the lanes
```
allowedEdgeTypes = {"highway.primary", "highway.primary_link", "highway.secondary", "highway.secondary_link",
                       "highway.tertiary", "highway.tertiary_link", "highway.unclassified", "highway.residential",
                       "highway.living_street", "highway.cycleway" }
allowedVehicleClasses = set(['private', 'bicycle', 'moped', 'motorcycle', 'evehicle'])
```
These filters allow you to specify which lanes are allowed for e-scooters.

## Create PoI for the charging stations:
A POI is created for each charging station to display in the SUMO GUI.
```
poi = sumolib.shapes.poi.PoI('poi_'+str(i), 'station', '4', sumolib.color.decodeXML('red'), xy[0], xy[1] )
```

