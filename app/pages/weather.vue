<template>
  <div class="min-h-screen bg-[url('/images/img.jpg')] bg-cover bg-center bg-no-repeat flex items-center justify-center p-4">
    <div
      class="w-full max-w-md rounded-3xl shadow-xl p-6 bg-cover bg-center"
      style="background-image: url('/images/bgimg.jpg');"
    >
      <h1 class="text-3xl font-bold text-center mb-6">
        🌤 Weather App
      </h1>

      <div v-if="loading" class="text-center py-10">
        Loading...
      </div>

      <div v-if="weather && !loading">

        <div class="text-center">

          <div class="text-6xl mb-2">
            {{ weatherEmoji }}
          </div>

          <div class="text-5xl font-bold">
            {{ weather.current.temperature_2m }}°C
          </div>

          <div class="text-xl font-semibold mt-2">
            {{ location }}
          </div>

          <div class="text-gray-500">
            {{ weatherDescription }}
          </div>

        </div>

        <hr class="my-6">

        <h2 class="font-bold text-lg mb-3">
          Tomorrow
        </h2>

        <div class="bg-sky-50 rounded-xl p-4">
          <div class="flex justify-between">
            <span>High</span>
            <span>{{ weather.daily.temperature_2m_max[1] }}°C</span>
          </div>

          <div class="flex justify-between mt-2">
            <span>Low</span>
            <span>{{ weather.daily.temperature_2m_min[1] }}°C</span>
          </div>
        </div>

        <hr class="my-6">

        <h2 class="font-bold text-lg mb-4">
          7-Day Forecast
        </h2>

        <div
          v-for="(day, index) in weather.daily.time"
          :key="day"
          class="flex justify-between py-2 border-b"
        >
          <span>{{ formatDate(day) }}</span>

          <span>
            {{ weather.daily.temperature_2m_max[index] }}°
            /
            {{ weather.daily.temperature_2m_min[index] }}°
          </span>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from "vue"

const location = ref("Manila")
const weather = ref<any>(null)
const loading = ref(false)

const weatherDescription = computed(() => {
  const code = weather.value?.current.weather_code

  const map: Record<number, string> = {
    0: "Clear Sky",
    1: "Mainly Clear",
    2: "Partly Cloudy",
    3: "Cloudy",
    45: "Fog",
    48: "Fog",
    51: "Drizzle",
    61: "Rain",
    63: "Rain",
    65: "Heavy Rain",
    71: "Snow",
    80: "Rain Showers",
    95: "Thunderstorm"
  }

  return map[code] || "Unknown"
})

const weatherEmoji = computed(() => {
  const code = weather.value?.current.weather_code

  if (code === 0) return "☀️"
  if (code <= 3) return "⛅"
  if (code <= 55) return "🌫️"
  if (code <= 67) return "🌧️"
  if (code <= 77) return "❄️"
  if (code <= 82) return "🌦️"
  if (code >= 95) return "⛈️"

  return "☁️"
})

function formatDate(date: string) {
  return new Date(date).toLocaleDateString("en-US", {
    weekday: "short"
  })
}

async function searchWeather() {
  loading.value = true

  try {
    const geo = await fetch(
      "https://geocoding-api.open-meteo.com/v1/search?name=Manila&count=1"
    ).then(r => r.json())

    if (!geo.results?.length) {
      alert("City not found")
      loading.value = false
      return
    }

    const place = geo.results[0]
    location.value = place.name

    const data = await fetch(
      `https://api.open-meteo.com/v1/forecast?latitude=${place.latitude}&longitude=${place.longitude}&current=temperature_2m,weather_code&daily=temperature_2m_max,temperature_2m_min&timezone=auto`
    ).then(r => r.json())

    weather.value = data
  } catch (e) {
    alert("Unable to load weather.")
  }

  loading.value = false
}

onMounted(searchWeather)
</script>

<style>
body {
  margin: 0;
  font-family: Inter, Arial, sans-serif;
}
</style>