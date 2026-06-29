<template>
  <div>
    <h1>{{ currentWeather?.location?.name }}</h1>
    <h2>{{ currentWeather?.current?.temp_c }}</h2>
    

    <v-btn 
    @click="getWeatherData">get weather data</v-btn>
  </div>
</template>

<script lang="ts" setup>
//@ts-nocheck
definePageMeta({
  layout: 'weather',
  middleware: 'auth'
})

const currentWeather = ref(null);

const getWeatherData = async () => {
  try {
    const data = await $fetch('http://api.weatherapi.com/v1/current.json?key=20db62de8d274a0f85232651262906&q=manila&aqi=no', {
    })
    currentWeather.value = data
    console.log(data)
  } catch (error) {
    console.error('Failed to fetch',error)
  }
}

onMounted(getWeatherData)
</script>

<style>



</style>