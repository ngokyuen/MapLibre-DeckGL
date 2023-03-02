<template>
  <!-- <v-container class="fill-height fill-width"> -->
  <!-- <v-responsive class="d-flex align-center text-center fill-height"> -->
  <div id="map" style="width: 100%; height: 100%"></div>
  <!-- </v-responsive> -->
  <!-- </v-container> -->
</template>

<script setup>
import { onMounted } from "vue";

import { Tile3DLayer } from "@deck.gl/geo-layers";
import { Tiles3DLoader } from "@loaders.gl/3d-tiles";

import { MapboxLayer } from "@deck.gl/mapbox";

function addControls(map) {
  map.addControl(
    new maplibregl.NavigationControl({
      visualizePitch: true,
      showZoom: true,
      showCompass: true,
    })
  );
}

function addSources(map) {
  map.addSource("hkImagery", {
    type: "raster",
    tiles: [
      // "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/imagery/WGS84/{z}/{x}/{y}.png",
      "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/basemap/WGS84/{z}/{x}/{y}.png",
    ],
    tileSize: 256,
    attribution: "Map & Data from Lands Department (HK Gov)",
  });
  map.addSource("hkLabelEN", {
    type: "raster",
    tiles: [
      "https://mapapi.geodata.gov.hk/gs/api/v1.0.0/xyz/label/hk/en/WGS84/{z}/{x}/{y}.png",
    ],
    tileSize: 256,
  });
}

function addLayers(map) {
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

  const building = new MapboxLayer({
    id: "building",
    type: Tile3DLayer,
    data: "https://services.landsd.gov.hk/3d-data/3dtiles/2021_ke/tileset.json",
    loader: Tiles3DLoader,
    loadOptions: {},
    onTileError: (err) => {
      console.log(err);
    },
    onTilesetLoader: (tileset) => {
      // console.log(tileset);
    },
  });

  map.addLayer(building);
}

onMounted(() => {
  var map = new maplibregl.Map({
    container: "map",
    style: "https://demotiles.maplibre.org/style.json", // stylesheet location
    center: [114.177216, 22.302711], // starting position [lng, lat]
    zoom: 10, // starting zoom,
  });

  console.log("map: ", map);

  map.on("load", () => {
    // console.log("map load")
    addControls(map);
    addSources(map);
    addLayers(map);
  });
});
</script>
