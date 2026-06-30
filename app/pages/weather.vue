```vue
<template>
  <div class="weather-page">
    <!-- Background Overlay -->
    <div class="overlay"></div>

    <div class="container">
      <!-- Main Weather Card -->
      <div class="weather-card">
        <div class="weather-content">
          <div class="left">
            <p class="country">
              📍 {{ currentWeather?.location?.country }}
            </p>

            <h1 class="city">
              {{ currentWeather?.location?.name }}
            </h1>

            <div class="temperature">
              {{ currentWeather?.current?.temp_c }}°
            </div>

            <p class="condition">
              {{ currentWeather?.current?.condition?.text }}
            </p>

            <p class="feels">
              Feels like {{ currentWeather?.current?.feelslike_c }}°
            </p>

            <v-btn
              class="weather-btn"
              @click="getWeatherData"
            >
              Refresh Weather
            </v-btn>
          </div>

          <!-- Weather Icon -->
          <div class="right">
            <img
              :src="currentWeather?.current?.condition?.icon"
              alt="weather icon"
              class="weather-icon"
            />
          </div>
        </div>

        <!-- Bottom Stats -->
        <div class="stats">
          <div class="stat-box">
            <span>💧 Humidity</span>
            <h3>{{ currentWeather?.current?.humidity }}%</h3>
          </div>

          <div class="stat-box">
            <span>🌬 Wind</span>
            <h3>{{ currentWeather?.current?.wind_kph }} km/h</h3>
          </div>

          <div class="stat-box">
            <span>🧭 Direction</span>
            <h3>{{ currentWeather?.current?.wind_dir }}</h3>
          </div>

          <div class="stat-box">
            <span>☁ Cloud</span>
            <h3>{{ currentWeather?.current?.cloud }}%</h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
// @ts-nocheck

definePageMeta({
  layout: 'weather',
  middleware: 'auth'
})

const currentWeather = ref(null)

const getWeatherData = async () => {
  try {
    const data = await $fetch(
      'http://api.weatherapi.com/v1/current.json?key=20db62de8d274a0f85232651262906&q=manila&aqi=no'
    )

    currentWeather.value = data
  } catch (error) {
    console.error('Failed to fetch', error)
  }
}

onMounted(getWeatherData)
</script>

<style scoped>
.weather-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #0f172a, #2563eb);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 20px;
  position: relative;
  overflow: hidden;
}

.overlay {
  position: absolute;
  inset: 0;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
}

.container {
  width: 100%;
  max-width: 1200px;
  position: relative;
  z-index: 2;
}

.weather-card {
  background: rgba(255, 255, 255, 0.12);
  border-radius: 30px;
  padding: 40px;
  backdrop-filter: blur(20px);
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  color: white;
}

.weather-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
}

.left {
  flex: 1;
}

.country {
  font-size: 18px;
  opacity: 0.9;
  margin-bottom: 10px;
}

.city {
  font-size: 64px;
  font-weight: 700;
  margin-bottom: 10px;
}

.temperature {
  font-size: 120px;
  font-weight: bold;
  line-height: 1;
}

.condition {
  font-size: 28px;
  margin-top: 10px;
  font-weight: 500;
}

.feels {
  font-size: 18px;
  opacity: 0.8;
  margin-top: 10px;
}

.weather-btn {
  margin-top: 30px;
  background: white !important;
  color: #2563eb !important;
  font-weight: bold;
  border-radius: 12px !important;
  padding: 0 24px !important;
  height: 50px !important;
}

.right {
  display: flex;
  justify-content: center;
  align-items: center;
}

.weather-icon {
  width: 220px;
  height: 220px;
}

.stats {
  margin-top: 40px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 20px;
}

.stat-box {
  background: rgba(255, 255, 255, 0.15);
  padding: 24px;
  border-radius: 20px;
  text-align: center;
  backdrop-filter: blur(10px);
}

.stat-box span {
  display: block;
  font-size: 16px;
  margin-bottom: 10px;
  opacity: 0.9;
}

.stat-box h3 {
  font-size: 30px;
  font-weight: bold;
}

@media (max-width: 768px) {
  .weather-card {
    padding: 24px;
  }

  .city {
    font-size: 42px;
  }

  .temperature {
    font-size: 80px;
  }

  .weather-icon {
    width: 140px;
    height: 140px;
  }

  .weather-content {
    flex-direction: column;
    text-align: center;
  }
}
</style>
```
