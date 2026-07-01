<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-sky-100 p-6">

    <h1 class="text-xl sm:text-2xl font-bold mb-6">
      Weather App
    </h1>

    <div v-if="loading">
      Loading...
    </div>

    <div v-if="weather && !loading" class="text-center w-full">

      <!-- CURRENT WEATHER -->
      <div class="text-6xl sm:text-7xl mb-2">
        {{ weatherEmoji }}
      </div>

      <div class="text-4xl sm:text-5xl font-bold">
        {{ weather.current.temp_c }}°C
      </div>

      <div class="text-lg sm:text-xl text-gray-700 mt-1">
        {{ location }}
      </div>

      <div class="text-gray-600 mb-6">
        {{ weather.current.condition.text }}
      </div>

      <!-- FUTURE WEATHER -->
      <h2 class="font-bold text-lg mb-3">
        Next Hours
      </h2>

      <div class="w-full max-w-2xl mx-auto space-y-2">

        <div
          v-for="(hour, index) in futureHours"
          :key="index"
          class="flex justify-between border-b py-2 text-sm sm:text-base"
        >
          <span>{{ formatHour(hour.time) }}</span>
          <span>{{ hour.temp_c }}°C</span>
        </div>

      </div>

    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from "vue"

const weather = ref<any>(null)
const location = ref("")
const loading = ref(false)

const API_KEY = "20db62de8d274a0f85232651262906"

// Emoji
const weatherEmoji = computed(() => {
  const text = weather.value?.current?.condition?.text?.toLowerCase() || ""

  if (text.includes("sun")) return "☀️"
  if (text.includes("cloud")) return "⛅"
  if (text.includes("rain")) return "🌧️"
  if (text.includes("storm")) return "⛈️"
  if (text.includes("snow")) return "❄️"

  return "☁️"
})

// Future hours
const futureHours = computed(() => {
  return weather.value?.forecast?.forecastday?.[0]?.hour || []
})

// Format time
function formatHour(datetime: string) {
  return new Date(datetime).toLocaleTimeString("en-US", {
    hour: "2-digit",
    minute: "2-digit"
  })
}

async function loadWeather() {
  loading.value = true

  const res = await fetch(
    `https://api.weatherapi.com/v1/forecast.json?key=${API_KEY}&q=Manila&days=1&aqi=no&alerts=no`
  )

  const data = await res.json()

  location.value = `${data.location.name}, ${data.location.country}`
  weather.value = data

  loading.value = false
}

onMounted(loadWeather)
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
}
</style>