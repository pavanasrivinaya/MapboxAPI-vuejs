<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id="map"></div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  data() {
    return {
      message: "MapBox API"
    };
  },
  mounted: function() {
    // eslint-disable-next-line no-undef
    mapboxgl.accessToken =
      "pk.eyJ1IjoicGFyaXNyaSIsImEiOiJja2ppNXpmaHUxNmIwMnpsbzd5YzczM2Q1In0.8VJaqwqZ_zh8qyeAuqWQgw";
    // eslint-disable-next-line no-undef
    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [-79.4512, 43.6568],
      zoom: 13
    });

    map.addControl(
      // eslint-disable-next-line no-undef
      new MapboxGeocoder({
        // eslint-disable-next-line no-undef
        accessToken: mapboxgl.accessToken,
        // eslint-disable-next-line no-undef
        mapboxgl: mapboxgl
      })
    );
  }
};
</script>
<style>
body {
  margin: 0;
  padding: 0;
}
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  height: 700px;
  width: 100%;
  margin-top: 147px;
}
</style>
