<template>
  <l-map
    style="height: 100%; width: 100%"
    :zoom="zoom"
    :center="center"
    :options="{zoomControl: false}"
  >
    <l-tile-layer :url="url"></l-tile-layer>
  </l-map>
</template>

<script>
Vue.component("l-map", Vue2Leaflet.LMap);
Vue.component("l-tile-layer", Vue2Leaflet.LTileLayer);

export default {
  data() {
    return {
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 8,
      center: [47.31322, -1.319482]
    };
  }
};
</script>