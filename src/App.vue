<template>
  <div id="app">
    <Map></Map>
  </div>
</template>

<script>
import Map from "./components/Map";

export default {
  name: "App",
  // el: "#togglerLightMode",
  components: {
    Map
  }
};
</script>

<style>
@import "../node_modules/leaflet/dist/leaflet.css";

.leaflet-fake-icon-image-2x {
  background-image: url(../node_modules/leaflet/dist/images/marker-icon-2x.png);
}
.leaflet-fake-icon-shadow {
  background-image: url(../node_modules/leaflet/dist/images/marker-shadow.png);
}
body {
  margin: 0px;
  font-family: Helvetica, Verdana, sans-serif;
}
</style>
