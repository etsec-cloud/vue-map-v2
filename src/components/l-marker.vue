<template>
  <l-map style="height: 80%; width: 100%" :zoom="zoom" :center="center">
    <l-tile-layer :url="url"></l-tile-layer>
    <l-marker :lat-lng="markerLatLng"></l-marker>
  </l-map>
</template>

<script>
Vue.component("l-map", Vue2Leaflet.LMap);
Vue.component("l-tile-layer", Vue2Leaflet.LTileLayer);
Vue.component("l-marker", Vue2Leaflet.LMarker);

export default {
  data() {
    return {
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 3,
      center: [47.41322, -1.219482],
      markerLatLng: [47.31322, -1.319482]
    };
  }
};
</script>