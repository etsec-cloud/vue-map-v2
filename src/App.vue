<template>
  <div id="app">
    <Map></Map>
    <popUp></popUp>
  </div>
</template>

<script>
/* eslint no-console: "off" */
import Map from "./components/Map";
import popUp from "./components/popUp";
import axios from "axios";

export default {
  name: "App",
  components: {
    Map,
    popUp
  },
  data() {
    return {
      pingsAPI: null
    };
  },
  mounted() {
    // Charge cette partie lorsque App est entièrement chargé
    axios
      .get("https://ionia-server.herokuapp.com/api/v1/events")
      .then(response => console.log(response))
      .catch(error => console.log(error));
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
