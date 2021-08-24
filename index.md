<html lang="en">
<head>
    <meta charset="utf-8">
    <title>OpenStreetMap &amp; OpenLayers - Marker Example</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
    <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
<script type='text/javascript' src='https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js'></script>

  <script>
    /* OSM & OL example code provided by https://mediarealm.com.au/ */
    
    
    







  // load the data

  
    var p;
    var map;
       var mapLat = 0;  

    var mapLng = 0;

    var mapDefaultZoom = 18;
    

function nachladen() {
var http = null;
if (window.XMLHttpRequest) {
http = new XMLHttpRequest();
} else if (window.ActiveXObject) {
http = new ActiveXObject("Microsoft.XMLHTTP");
}






if (http != null) {
http.open("GET", "https://api.thingspeak.com/channels/1300758/fields/1/last.html", true);
http.onreadystatechange = ausgeben;

http.send(null);
}
function ausgeben() {
if (http.readyState == 4) {

document.getElementById("Ausgabe").innerHTML =
http.responseText ;
 mapLat = parseFloat(http.responseText);

}



}
}


 function nachladen2() {
var http = null;
if (window.XMLHttpRequest) {
http = new XMLHttpRequest();
} else if (window.ActiveXObject) {
http = new ActiveXObject("Microsoft.XMLHTTP");
}
if (http != null) {
http.open("GET", "https://api.thingspeak.com/channels/1300758/fields/2/last.html", true);
http.onreadystatechange = ausgeben2;

http.send(null);
}
function ausgeben2() {
if (http.readyState == 4) {

document.getElementById("Ausgabe2").innerHTML =
http.responseText ;
 mapLng = parseFloat(http.responseText);
 
initialize_map();  
add_map_point(mapLat,mapLng); 


}
}
}   

    

  
 
 
  
    
    function initialize_map() {
    
    

    
    
 
//alert("Lvalor: "+p) 
    
    
      map = new ol.Map({
        target: "map",
        layers: [
            new ol.layer.Tile({
                source: new ol.source.OSM({
                      url: "https://a.tile.openstreetmap.org/{z}/{x}/{y}.png"
                })
            })
        ],
        view: new ol.View({
            center: ol.proj.fromLonLat([mapLng, mapLat]),
            zoom: mapDefaultZoom
        })
      
 
      });
      
      
      
      
    }
    function add_map_point(lat, lng) {
      var vectorLayer = new ol.layer.Vector({
        source:new ol.source.Vector({
          features: [new ol.Feature({
                geometry: new ol.geom.Point(ol.proj.transform([parseFloat(lng), parseFloat(lat)], 'EPSG:4326', 'EPSG:3857')),
            })]
        }),
        style: new ol.style.Style({
          image: new ol.style.Icon({
            anchor: [0.5, 0.5],
            anchorXUnits: "fraction",
            anchorYUnits: "fraction",
            src: "https://upload.wikimedia.org/wikipedia/commons/e/ec/RedDot.svg"
          })
        })
            
      });

    map.addLayer(vectorLayer); 

    }






   

setInterval('nachladen()', 100);
setInterval('nachladen()', 100);


  </script>
</head>
<body onload="nachladen(); nachladen2();

">
    


  
<body> 

<div id="map" style="width: 50vw; height: 50vh;"></div>

<div id="Ausgabe"></div> <div id="Ausgabe2"></div>

</body>