<template>
  <div class="home">
    <!-- <p>{{ location }}</p> -->

    <h1>{{ message }}</h1>
    <div id="map"></div>
    <div id="geocoder" class="geocoder"></div>

    <pre id="coordinates" class="coordinates"></pre>
    <div id="menu">
      <input
        id="streets-v11"
        type="radio"
        name="rtoggle"
        value="streets"
        checked="checked"
      />
      <label for="streets-v11">streets</label>
      <input id="light-v10" type="radio" name="rtoggle" value="light" />
      <label for="light-v10">light</label>
      <input id="dark-v10" type="radio" name="rtoggle" value="dark" />
      <label for="dark-v10">dark</label>
      <input id="outdoors-v11" type="radio" name="rtoggle" value="outdoors" />
      <label for="outdoors-v11">outdoors</label>
      <input id="satellite-v9" type="radio" name="rtoggle" value="satellite" />
      <label for="satellite-v9">satellite</label>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  data() {
    return {
      // location: [],
      message: "MapBox API"
    };
  },
  mounted: function() {
    // eslint-disable-next-line no-undef
    mapboxgl.accessToken =
      "pk.eyJ1IjoicGFyaXNyaSIsImEiOiJja2ppNXpmaHUxNmIwMnpsbzd5YzczM2Q1In0.8VJaqwqZ_zh8qyeAuqWQgw";
    var coordinates = document.getElementById("coordinates");

    // eslint-disable-next-line no-undef
    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [80.18536880746353, 16.501575031841256],
      zoom: 13
    });
    // Add zoom and rotation controls to the map.
    // eslint-disable-next-line no-undef
    map.addControl(new mapboxgl.NavigationControl());
    // eslint-disable-next-line no-undef
    map.addControl(new mapboxgl.FullscreenControl());
    map.addControl(
      // eslint-disable-next-line no-undef
      new mapboxgl.GeolocateControl({
        positionOptions: {
          enableHighAccuracy: true
        },
        trackUserLocation: true
      })
    );
    // eslint-disable-next-line no-undef
    var marker = new mapboxgl.Marker({
      draggable: true
    })
      .setLngLat([0, 0])
      .addTo(map);
    // function setLocationCoordinates(lngLat) {
    //   this.location = [lngLat.lng, lngLat.lat];
    // }
    // marker.on(" setLocationCoordinates", setLocationCoordinates);
    function onDragEnd() {
      var lngLat = marker.getLngLat();
      coordinates.style.display = "block";
      coordinates.innerHTML =
        "Longitude: " + lngLat.lng + "<br />Latitude: " + lngLat.lat;
    }
    marker.on("dragend", onDragEnd);
    var layerList = document.getElementById("menu");
    var inputs = layerList.getElementsByTagName("input");

    function switchLayer(layer) {
      var layerId = layer.target.id;
      map.setStyle("mapbox://styles/mapbox/" + layerId);
    }

    for (var i = 0; i < inputs.length; i++) {
      inputs[i].onclick = switchLayer;
    }

    // eslint-disable-next-line no-undef
    var geocoder = new MapboxGeocoder({
      // eslint-disable-next-line no-undef
      accessToken: mapboxgl.accessToken,
      // eslint-disable-next-line no-undef
      mapboxgl: mapboxgl
    });

    document.getElementById("geocoder").appendChild(geocoder.onAdd(map));
  }
  // setLocationCoordinates(lngLat) {
  //   this.location = [lngLat.lng, lngLat.lat];
  // }
};
</script>
<style>
#menu {
  position: absolute;
  background: #fff;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
}
body {
  margin: 0;
  padding: 0;
}
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  height: 500px;
  width: 100%;
  margin-top: 147px;
}
.coordinates {
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  position: absolute;
  bottom: 40px;
  left: 10px;
  padding: 5px 10px;
  margin: 0;
  font-size: 11px;
  line-height: 18px;
  border-radius: 3px;
  display: none;
}
.geocoder {
  position: absolute;
  z-index: 1;
  width: 50%;
  left: 50%;
  margin-left: 15%;
  top: 97px;
}
.mapboxgl-ctrl-geocoder {
  min-width: 100%;
}
</style>
