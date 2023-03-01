<template>
  <!-- <v-container class="fill-height fill-width"> -->
  <!-- <v-responsive class="d-flex align-center text-center fill-height"> -->
  <div id="map" style="width: 100%; height: 100%"></div>
  <!-- </v-responsive> -->
  <!-- </v-container> -->
</template>

<script setup>
import { onMounted } from "vue";
onMounted(() => {
  var map = new maplibregl.Map({
    container: "map",
    style: "https://demotiles.maplibre.org/style.json", // stylesheet location
    center: [114.177216, 22.302711], // starting position [lng, lat]
    zoom: 10, // starting zoom,
  });

  console.log(map);

  map.on("load", () => {
    // console.log("map load")
    map.addSource("hkImagery", {
      type: "raster",
      tiles: [
        // "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/imagery/WGS84/{z}/{x}/{y}.png",
        "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/basemap/WGS84/{z}/{x}/{y}.png",
      ],
      tileSize: 256,
    });
    map.addSource("hkLabelEN", {
      type: "raster",
      tiles: [
        "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/label/hk/en/WGS84/{z}/{x}/{y}.png",
      ],
      tileSize: 256,
    });

    map.addLayer({
      id: "hkImageryLayer",
      type: "raster",
      source: "hkImagery",
    });
    map.addLayer({
      id: "hkLabelENLayer",
      type: "raster",
      source: "hkLabelEN",
    });
  });
});
</script>
