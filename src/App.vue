<template>
  <header>
    <div></div>
  </header>

  <main>
    <div id="mapContainer"></div>
    <div>
      <ul>
        <li v-for="cursa in cursesData" :key="cursa.id">{{ cursa.nom }}</li>
      </ul>
    </div>
  </main>
</template>

<script>
import { toRaw } from "vue";
import L from "leaflet";
import "leaflet.markercluster";
import arxiuCurses from "./assets/curses2022.json";
export default {
  name: "App",
  data() {
    return {
      center: [40.436615, -2.369812],
      map: null,
      markers: null,
      cursesData: arxiuCurses,
      cluster: null,
    };
  },
  methods: {
    setupMarkers() {
      toRaw(this.markers).clearLayers();
      this.cursesData.forEach((cursa) => this.ficaMarkers(cursa));
      toRaw(this.map).addLayer(toRaw(this.markers));
    },
    setupLeafletMap() {
      this.map = L.map("mapContainer").setView(this.center, 6);

      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: "OpenStreetMap",
      }).addTo(toRaw(this.map));

      this.markers = L.markerClusterGroup({
        /*unspiderfy: false*/
      });
    },
    ficaMarkers(cursa) {
      return toRaw(this.markers).addLayer(
        L.marker([cursa.coordenades[0], cursa.coordenades[1]], {
          title: cursa.nom,
        }).bindPopup(cursa.distancies)
      );
    },
  },
  mounted() {
    this.setupLeafletMap();
    this.setupMarkers();
  },
};
</script>




<style>
#mapContainer {
  height: 600px;
}
</style>
