<template>
  <div id="mymap" class="flex-container">
    <div class="position1">
      <div class="searchBar">
        <form class="search-container">
          <input type="search" id="site-search" name="q" aria-label="Search through site content" />
          <a href="#">
            <div class="search-icon" id="logoLoupe">

            </div>
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
    <p class="francisporc" id="texte">France Esport</p>

    <l-map
      :zoom.sync="zoom"
      :options="mapOptions"
      :center="center"
      :bounds="bounds"
      :min-zoom="minZoom"
      :max-zoom="maxZoom"
    >
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
        <l-polyline
          v-for="item in polylines"
          :key="item.id"
          :lat-lngs="item.points"
          :visible="item.visible"
          @click="alert(item)"
        />
      </l-layer-group>
      <l-layer-group
        v-for="item in stuff"
        :key="item.id"
        :visible="item.visible"
        layer-type="overlay"
        name="Layer 1"
      >
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
        <l-polyline
          :lat-lngs="item.polyline.points"
          :visible="item.polyline.visible"
          @click="alert(item.polyline)"
        />
      </l-layer-group>
    </l-map>
  </div>
</template>

<script>

let currentLightMode = "Light";
import { icon, latLngBounds } from "leaflet";
import {
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
} from "vue2-leaflet";

const markers1 = [
  // {
  //   position: { lng: -1.219482, lat: 47.41322 },
  //   visible: true,
  //   draggable: true
  // },
  // { position: { lng: -1.571045, lat: 47.457809 } },
  // { position: { lng: -1.560059, lat: 47.739323 } },
  // { position: { lng: -0.922852, lat: 47.886881 } },
  // { position: { lng: -0.769043, lat: 48.231991 } },
  // { position: { lng: 0.395508, lat: 48.268569 } },
  // { position: { lng: 0.604248, lat: 48.026672 } },
  // { position: { lng: 1.2854, lat: 47.982568 } },
  // { position: { lng: 1.318359, lat: 47.894248 } },
  // { position: { lng: 1.373291, lat: 47.879513 } },
  // { position: { lng: 1.384277, lat: 47.798397 } },
  // { position: { lng: 1.329346, lat: 47.754098 } },
  // { position: { lng: 1.329346, lat: 47.680183 } },
  // { position: { lng: 0.999756, lat: 47.635784 } },
  // { position: { lng: 0.86792, lat: 47.820532 } },
  // { position: { lng: 0.571289, lat: 47.820532 } },
  // { position: { lng: 0.439453, lat: 47.717154 } },
  // { position: { lng: 0.439453, lat: 47.61357 } },
  // { position: { lng: -0.571289, lat: 47.487513 } },
  // { position: { lng: -0.615234, lat: 47.680183 } },
  // { position: { lng: -0.812988, lat: 47.724545 } },
  // { position: { lng: -1.054688, lat: 47.680183 } },
  // { position: { lng: -1.219482, lat: 47.41322 } }
];

// const poly1 = [
//   { lng: -1.219482, lat: 47.41322 },
//   { lng: -1.571045, lat: 47.457809 },
//   { lng: -1.560059, lat: 47.739323 },
//   { lng: -0.922852, lat: 47.886881 },
//   { lng: -0.769043, lat: 48.231991 },
//   { lng: 0.395508, lat: 48.268569 }
// ];

let tileProviders = [
  {
    name: "OpenStreetMap",
    visible: true,
    attribution:
      '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
    url:
      "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png"
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
        // {
        //   id: "m2",
        //   position: { lat: 51.8905, lng: -0.09 },
        //   tooltip: "tooltip for marker2",
        //   draggable: true,
        //   visible: false
        // },
        // {
        //   id: "m3",
        //   position: { lat: 51.005, lng: -0.09 },
        //   tooltip: "tooltip for marker3",
        //   draggable: true,
        //   visible: true
        // },
        // {
        //   id: "m4",
        //   position: { lat: 50.7605, lng: -0.09 },
        //   tooltip: "tooltip for marker4",
        //   draggable: true,
        //   visible: false
        // }
      ],
      polylines: [
        // {
        //   id: "p1",
        //   points: [
        //     { lat: 37.772, lng: -122.214 },
        //     { lat: 21.291, lng: -157.821 },
        //     { lat: -18.142, lng: -181.569 },
        //     { lat: -27.467, lng: -206.973 }
        //   ],
        //   visible: true
        // },
        // {
        //   id: "p2",
        //   points: [[-73.91, 40.78], [-87.62, 41.83], [-96.72, 32.76]],
        //   visible: true
        // }
      ],
      // stuff: [
      //   {
      //     id: "s1",
      //     markers: markers1,
      //     polyline: { points: poly1, visible: false },
      //     visible: true,
      //     markersVisible: true
      //   }
      // ],
      bounds: latLngBounds({ lat: 48.2, lng: 4.5 }, { lat: 51, lng: 0 })
    };
  },


  
  
 


  methods: {
    // alert(item) {
    //   alert("this is " + JSON.stringify(item));
    // },


    clickers: function() {
const texte = document.getElementById('texte');
const searchBar = document.getElementById('site-search');
const loupe = document.getElementById('logoLoupe');


      switch (currentLightMode) {
        case "Light":
          currentLightMode = "Dark";
          tileProviders[0].url =
            "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png";
          texte.classList.toggle("dark");
          searchBar.classList.toggle("noir");
          loupe.classList.toggle("search-icon-w")

          break;

        case "Dark":
          currentLightMode = "Light";
          tileProviders[0].url =
            "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png";
          texte.classList.toggle("dark");
          searchBar.classList.toggle("noir");
          loupe.classList.toggle("search-icon-w")

          break;

        default:
          currentLightMode = "Light";
          tileProviders[0].url =
            "	https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png";


            
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
    },
    fitPolyline: function() {
      const bounds = latLngBounds(markers1.map(o => o.position));
      this.bounds = bounds;
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
