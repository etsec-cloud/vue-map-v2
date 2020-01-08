<template>
  <div id="app">
    <Example></Example>
  </div>
</template>

<script>
import Example from "./components/Example";

export default {
  name: "App",
  components: {
    Example
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
