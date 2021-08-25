<html>
  <head>
    <title>shiyaztech</title>
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no">
    <meta charset="utf-8">
    <style>
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
        height: 100%;
      }
      /* Optional: Makes the sample page fill the window. */
      html, body {
        height: 100%;
        margin: 0;
        padding: 0;
      }
    </style>
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_Google_MAPS_API_KEY"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <script>

      var map;
        var x;
        function loadmaps(){
           $.getJSON("https://api.thingspeak.com/channels/1300758/fields/2/last.json?api_key=NLX7MNKDEMTXVNTD", function(result){
        	
            var m = result;
            x=Number(m.field2);
                          // alert(x);

        });
          $.getJSON("https://api.thingspeak.com/channels/1300758/fields/1/last.json?api_key=NLX7MNKDEMTXVNTD", function(result){
        	
            var m = result;
            y=Number(m.field1);
                   // alert(y);
                
        }).done(function() {
            
                initialize();
    });
            
        }
        window.setInterval(function(){
        loadmaps();
            }, 9000);
      function initialize() {
          //alert(y);
        var mapOptions = {
          zoom: 18,
          center: {lat: x, lng: y}
        };
        map = new google.maps.Map(document.getElementById('map'),
            mapOptions);

        var marker = new google.maps.Marker({
          position: {lat: x, lng: y},
          map: map
        });

        var infowindow = new google.maps.InfoWindow({
          content: '<p>Marker Location:' + marker.getPosition() + '</p>'
        });

        google.maps.event.addListener(marker, 'click', function() {
          infowindow.open(map, marker);
        });
      }

      google.maps.event.addDomListener(window, 'load', initialize);
    </script>
  </head>
  <body>
    <div id="map"></div>
  </body>
</html>
