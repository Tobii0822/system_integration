<script setup lang="ts">
import { ref, onMounted } from 'vue'

const mapContainer = ref<HTMLDivElement | null>(null)

onMounted(async () => {
  const L = await import('leaflet')

  if (!mapContainer.value) return

  const map = L.map(mapContainer.value).setView([14.5995, 120.9842], 13)

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
  }).addTo(map)
})
</script>

<template>
  <div ref="mapContainer" class="map"></div>
</template>

<style scoped>
.map {
  width: 1500px;
  height: 600px;
  
  
}
</style>