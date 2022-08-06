# Air_quality_Guatemala
This project was made with the purpose of creating different scripts in Google Earth Engine for air quality in Guatemala. These scripts can be changed to fulfill the purpose of the research, this means that you can chance either the imagery date, bands or other parameters. 


*Note. This is one of my first scripts, so there can be any error that can change the resulting data for analysis. 

#Methane script -spanish version-

//centrar
var centro=ee.Geometry.Point(-90.5328, 14.6362 );
Map.centerObject(centro,8);

//poligono
var polygon= ee.Geometry.Polygon([[-90.77799505002814,14.221134346410926]
,[-90.20670598752814,14.221134346410926]
,[-90.20670598752814,14.90432375402646]
,[-90.77799505002814,14.90432375402646]
,[-90.77799505002814,14.221134346410926]]);
Map.addLayer(polygon);

//escoger las imagenes a procesar de Sentinel 5 por medio de filtrado
var data = ee.ImageCollection("COPERNICUS/S5P/OFFL/L3_CH4")

var filter_region= data.filterBounds(polygon)

var filter_date= filter_region.filterDate('2021-01-01', '2021-12-31')
//escoger la banda que necesito, en este caso será la de la columna de densidad de CH4
var CH4_BAND = filter_date.select('CH4_column_volume_mixing_ratio_dry_air')
//se hace una media de todas las imagenes del año
var mean_CH4 = CH4_BAND.reduce(ee.Reducer.mean())
//visualizar el mapa con colores
var band_viz = {
  min: 1750,
  max: 1900,
  palette: ['black', 'blue', 'purple', 'cyan', 'green', 'yellow', 'red']
};

var clip_CH4= ee.Image(mean_CH4).clip(polygon)

Map.addLayer (clip_CH4, band_viz, 'S5P CH4')
var gt_CH4 = Map.addLayer
print (gt_CH4)

//Para exportar la imagen elimine el comentario de la siguiente sección
//Export.image.toDrive({
 // image: clip_CH4,
  //description: 'CH4_rast',
 // scale:30,
 // region: polygon,
  
//})
