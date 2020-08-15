
## **Using Earth Engine to make some geoprocessing analysis with DEM in Senegalese department named Podor** 
*1. Import Collection image*
```js
// SRTM Data
var srtm = ee.Image("CGIAR/SRTM90_V4"),
// JRC Data collection
    water = ee.Image("JRC/GSW1_0/GlobalSurfaceWater"),
    // Shapefile of the Podor locality in Senegal
    podor = ee.FeatureCollection("users/ndmorndiaye/dep_podor");
```
*2. Outline Podor limit*
```js
var limit=ee.Image().paint(podor, "black", 2);
```
*3. Clip Data with Podor limit*
```js
// Clip SRTM
srtm=srtm.clip(podor);
// Clip JRC 
water=water.clip(podor);
```
*4. Geoprocessing*
```js
// Slope
var slope = ee.Terrain.slope(srtm);
// Aspect 
var aspect=ee.Terrain.aspect(srtm);
```
*5. Display Maps and limit*
```js
Map.addLayer(srtm, {min:7.26, max:67.74,palette:"blue,yellow,green,red"}, 'srtm');
Map.addLayer(slope, {min:0.005692210197448731, max:0.27891829967498777},'slope');
Map.addLayer(aspect, {min:7.155878962343049, max:350.63806915480944},'aspect');
Map.addLayer(water, {bands:'occurrence', min:2.5, max:75.48, palette:'lightblue,blue'}, 'occurrence');
Map.addLayer(limit,{},"Podor");
Map.centerObject(podor,8)
```
