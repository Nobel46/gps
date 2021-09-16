<head>
    <meta charset="utf-8">
    <title>OpenStreetMap &amp; OpenLayers - Marker Example</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
    <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
    <script>
  
    var p;
    var map;
    var mapLat = 0;  
    var mapLng = 0;
    var mapDefaultZoom = 18;
    var act =0;
    var act2 =0;
    var channel_id ;
  
    
function Sumar() {
           channel_id = document.getElementById('txtN1').value;
         
          act2 =1;
            nachladen();
        }
function nachladen() {
var http = null;
if (window.XMLHttpRequest) {
http = new XMLHttpRequest();
} else if (window.ActiveXObject) {
http = new ActiveXObject("Microsoft.XMLHTTP");
}


if (http != null) {
http.open("GET", 'https://api.thingspeak.com/channels/'+ channel_id + '/fields/1/last.html', true);


http.onreadystatechange = ausgeben;

http.send(null);
}
function ausgeben() {
if (http.readyState == 4) {

document.getElementById("Ausgabe").innerHTML =
http.responseText ;
 mapLat = parseFloat(http.responseText);
nachladen2();
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
http.open("GET", 'https://api.thingspeak.com/channels/'+ channel_id + '/fields/2/last.html', true);
http.onreadystatechange = ausgeben2;

http.send(null);
}
function ausgeben2() {
if (http.readyState == 4) {

document.getElementById("Ausgabe2").innerHTML =
http.responseText ;
 mapLng = parseFloat(http.responseText);
 


nachladen3();
}
}
}   

    
function nachladen3() {
var http = null;
if (window.XMLHttpRequest) {
http = new XMLHttpRequest();
} else if (window.ActiveXObject) {
http = new ActiveXObject("Microsoft.XMLHTTP");
}
if (http != null) {
http.open("GET", 'https://api.thingspeak.com/channels/'+ channel_id + '/fields/3/last.html', true);
http.onreadystatechange = ausgeben3;

http.send(null);
}
function ausgeben3() {
if (http.readyState == 4) {

document.getElementById("Ausgabe3").innerHTML =
http.responseText ;

 


if(act == 0)
{
initialize_map();  



}

add_map_point(mapLat,mapLng); 


}
}
}   
  
 
 
  
    
    function initialize_map() {

      map =   new ol.Map({
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
    act=1;
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
   
if(act2==1)
{ setInterval('nachladen()', 100);}
    }


   ;

 
 
//
//window.onload=nachladen;

  </script>
</head>
<body onload="

">
 


<div id="map" style="width: 100vw; height: 100vh;"></div>  
<body> 




<div id="Ausgabe"></div> <div id="Ausgabe2"></div>
<div id="Ausgabe3"></div>
</body>

<fieldset>
        <legend>GPS</legend>
        <label for="">ID:</label>
        <input type="text" id="txtN1">
        <br><br>

        <input type="button" onclick="Sumar();" value="BUSCAR">
    </fieldset>

</body>
