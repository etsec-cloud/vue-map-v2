<template>
  <div id="mymap" class="flex-container">
    <div class="position1">
      <div class="searchBar">
        <form class="search-container">
          <input type="search" id="site-search" name="q" aria-label="Search through site content" />
          <a href="#">
            <div class="search-icon" id="logoLoupe"></div>
          </a>
        </form>
      </div>
    </div>

    <div class="bagoche">
      <label class="switch">
        <input type="checkbox" v-on:click="clickers()" />
        <span class="slider round"></span>
      </label>
    </div>
    <p class="francisporc" id="texte">©France Esport</p>

    <l-map :zoom.sync="zoom" :options="mapOptions" :center="center" :bounds="bounds" :min-zoom="minZoom" :max-zoom="maxZoom">
      <l-tile-layer
        v-for="tileProvider in tileProviders"
        :key="tileProvider.name"
        :name="tileProvider.name"
        :visible="tileProvider.visible"
        :url="tileProvider.url"
        :attribution="tileProvider.attribution"
        :token="token"
        layer-type="base"
      />
      <l-control-zoom :position="zoomPosition" />
      <l-control-attribution :position="attributionPosition" :prefix="attributionPrefix" />
      <l-control-scale :imperial="imperial" />
      <l-marker
        v-for="marker in markers"
        :key="marker.id"
        :visible="marker.visible"
        :draggable="marker.draggable"
        :lat-lng.sync="marker.position"
        :icon="marker.icon"
        @click="alert(marker)"
      >
        <l-popup :content="marker.tooltip" />
        <l-tooltip :content="marker.tooltip" />
      </l-marker>
      <l-layer-group layer-type="overlay" name="Layer polyline">
        <l-polyline v-for="item in polylines" :key="item.id" :lat-lngs="item.points" :visible="item.visible" @click="alert(item)" />
      </l-layer-group>
      <l-layer-group v-for="item in stuff" :key="item.id" :visible="item.visible" layer-type="overlay" name="Layer 1">
        <l-layer-group :visible="item.markersVisible">
          <l-marker
            v-for="marker in item.markers"
            :key="marker.id"
            :visible="marker.visible"
            :draggable="marker.draggable"
            :lat-lng="marker.position"
            @click="alert(marker)"
          />
        </l-layer-group>
        <l-polyline :lat-lngs="item.polyline.points" :visible="item.polyline.visible" @click="alert(item.polyline)" />
      </l-layer-group>
    </l-map>
  </div>
</template>

<script>
let currentLightMode = "Light";
import { icon, latLngBounds } from "leaflet";
import { LMap, LTileLayer, LMarker, LPolyline, LLayerGroup, LTooltip, LPopup, LControlZoom, LControlAttribution, LControlScale } from "vue2-leaflet";

// this.$parent.pingsAPI.forEach(element => {
//   /* eslint no-console: "off" */
//   console.log(element)
//   const newMarker = {
//     id: "m" + element.id,
//     position: { lat: element.lat, lng: element.lng },
//     draggable: false,
//     visible: true,
//     tooltip: element.title,
//     icon: icon.glyph({
//       prefix: "",
//       glyph: element.id
//     })
//   };

//   this.markers.push(newMarker);
// });

// let allMarkers;
let tileProviders = [
  {
    name: "OpenStreetMap",
    visible: true,
    attribution: '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
    url: "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png"
  }
];

// 	Dark mode : https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png
// Light mode : https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolyline,
    LLayerGroup,
    LTooltip,
    LPopup,
    LControlZoom,
    LControlAttribution,
    LControlScale
  },
  data() {
    return {
      center: [48.866667, 2.333333],
      opacity: 0.6,
      token: "your token if using mapbox",
      mapOptions: {
        zoomControl: false,
        attributionControl: false,
        zoomSnap: true
      },
      zoom: 8,
      minZoom: 1,
      maxZoom: 20,
      zoomPosition: "topleft",
      attributionPosition: "bottomright",
      layersPosition: "topright",
      attributionPrefix: "Vue2Leaflet",
      imperial: false,
      Positions: ["topleft", "topright", "bottomleft", "bottomright"],
      tileProviders: tileProviders,
      markers: [
        {
          id: "m1",
          position: { lat: 49.4431, lng: 1.0993 },
          tooltip: "LAN de Rouen",
          draggable: false,
          visible: true,
          icon: icon.glyph({
            prefix: "",
            glyph: "1"
          })
        },
        {
          id: "m2",
          position: { lat: 49.2534, lng: 4.033 },
          tooltip: "LAN de Reims",
          draggable: false,
          visible: true,
          icon: icon.glyph({
            prefix: "",
            glyph: "2"
          })
        }
      ],
      polylines: [],
      bounds: latLngBounds({ lat: 48.2, lng: 4.5 }, { lat: 51, lng: 0 })
    };
  },

  methods: {
    // alert(item) {
    //   alert("this is " + JSON.stringify(item));
    // },

    clickers: function() {
      const texte = document.getElementById("texte");
      const searchBar = document.getElementById("site-search");
      const loupe = document.getElementById("logoLoupe");

      switch (currentLightMode) {
        case "Light":
          currentLightMode = "Dark";
          // Change l'url d'obtention des tiles
          tileProviders[0].url = "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png";
          // Change la couleur des éléments
          texte.classList.toggle("dark");
          searchBar.classList.toggle("noir");
          loupe.classList.toggle("search-icon-w");

          break;

        case "Dark":
          currentLightMode = "Light";
          // Change l'url d'obtention des tiles
          tileProviders[0].url = "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png";
          4;
          // Change la couleur des éléments
          texte.classList.toggle("dark");
          searchBar.classList.toggle("noir");
          loupe.classList.toggle("search-icon-w");

          break;

        default:
          currentLightMode = "Light";
          tileProviders[0].url = "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png";
      }
    },
    addMarker: function() {
      const newMarker = {
        position: { lat: 50.5505, lng: -0.09 },
        draggable: true,
        visible: true
      };
      this.markers.push(newMarker);
    },
    removeMarker: function(index) {
      this.markers.splice(index, 1);
    }
  }
};
</script>
<style scoped>
@import "../../node_modules/leaflet/dist/leaflet.css";

@import "../assets/style.scss";

.leaflet-fake-icon-image-2x {
  background-image: url(../../node_modules/leaflet/dist/images/marker-icon-2x.png);
}
.leaflet-fake-icon-shadow {
  background-image: url(../../node_modules/leaflet/dist/images/marker-shadow.png);
}
body {
  margin: 0px;
}
#mymap {
  position: relative;
  height: 100vh;
}
</style>
