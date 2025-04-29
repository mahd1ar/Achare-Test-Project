
<script setup lang="ts">
import { ref, watch, onMounted, useTemplateRef } from "vue";
import  { Map, Marker } from 'maplibre-gl/dist/maplibre-gl.js';

// Props
interface LatLng {
  lat: number;
  lng: number;
}

const props = defineProps<{
  modelValue: LatLng;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: LatLng): void;
}>();

// Refs
const mapContainer = useTemplateRef("mapContainer");
const map = ref<Map | null>(null);
const marker = ref<Marker | null>(null);

const internalLat = ref(props.modelValue.lat);
const internalLng = ref(props.modelValue.lng);

// Methods
const updateLatLng = (lng: number, lat: number) => {
  internalLat.value = lat;
  internalLng.value = lng;
  emit("update:modelValue", { lat, lng });
};

const updateLatLngFromMarker = () => {
  if (marker.value) {
    const { lng, lat } = marker.value.getLngLat();
    updateLatLng(lng, lat);
  }
};

const initializeMap = () => {
  if (!mapContainer.value) return;

  map.value = new Map({
    container: mapContainer.value,
    style: "https://basemaps.cartocdn.com/gl/positron-gl-style/style.json",
    center: [internalLng.value, internalLat.value],
    zoom: 13,
  });

  marker.value = new Marker({ draggable: true })
    .setLngLat([internalLng.value, internalLat.value])
    .addTo(map.value);

  marker.value.on("dragend", updateLatLngFromMarker);

  map.value.on("click", (e) => {
    const { lng, lat } = e.lngLat;
    updateLatLng(lng, lat);
    // marker.value?.setLngLat([lng, lat]);

  });
};

// Watchers
watch(
  () => props.modelValue,
  (newValue) => {
    internalLat.value = newValue.lat;
    internalLng.value = newValue.lng;
    if (marker.value) {
      marker.value.setLngLat([newValue.lng, newValue.lat]);
    }
  },
  { immediate: true }
);

function setViewPosition() {
  navigator.geolocation.getCurrentPosition(
    (position) => {
      const { latitude, longitude } = position.coords;
      map.value?.flyTo({ center: [longitude, latitude], zoom: 13 });
      updateLatLng(longitude, latitude);
    },
    () => {
      console.log("User denied the request for Geolocation.");
    },
    {
      enableHighAccuracy: true,
      timeout: 5000,
      maximumAge: 0,
    }
  );

}

// Lifecycle
onMounted(() => {
  initializeMap();
});
</script>

<template>
  <div class="address-picker">
    <div class="bg-white" >
      <h6 class="container p-3 text-right" dir="rtl">
        لطفا موقعیت دقیق خود را بر روی نقشه مشخص کنید
      </h6>
    </div>
    <div class="map-container mb-3" ref="mapContainer"></div>
    <button class="current-location" @click="setViewPosition">
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M12 8c-2.21 0-4 1.79-4 4s1.79 4 4 4s4-1.79 4-4s-1.79-4-4-4m8.94 3A8.994 8.994 0 0 0 13 3.06V1h-2v2.06A8.994 8.994 0 0 0 3.06 11H1v2h2.06A8.994 8.994 0 0 0 11 20.94V23h2v-2.06A8.994 8.994 0 0 0 20.94 13H23v-2zM12 19c-3.87 0-7-3.13-7-7s3.13-7 7-7s7 3.13 7 7s-3.13 7-7 7"/></svg>
    </button>


  </div>
</template>

<style>
.map-container {
  width: 100%;
  height: 400px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.address-picker {

  position: relative;
}

.current-location {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background-color: white;
  padding: 5px;
  border-radius: 50%;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  border: none;
  cursor: pointer;
  width: 45px;
  height: 45px;
}
</style>
