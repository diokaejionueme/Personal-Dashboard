<script setup>
// Import Vue composition API functions
import { ref, onMounted } from 'vue'

// Reactive state for weather data and loading status
const weather = ref(null)
const loading = ref(true)

// Fetch weather data for Abuja from OpenWeatherMap API on mount
onMounted(async () => {
  const apiKey = import.meta.env.VITE_OPEN_WEATHER_API_KEY // API key from environment
  const city = 'Abuja'
  const response = await fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`
  )
  const data = await response.json()
  weather.value = data // Store weather data
  loading.value = false // Hide loading indicator
})
</script>
<template>

<!-- Weather widget UI -->
<div class="weather-widget">
  <div v-if="loading">Loading weather...</div>
  <!-- Note: 'error' is referenced but not defined in script -->
  <div v-else-if="error"> ⚠️{{ error }}</div>
  <div v-else>
    <!-- Display city, temperature, and weather description -->
    {{ weather.name }}: {{ weather.main.temp }}°C, 
    {{ weather.weather[0].description }}
  </div>
</div>
</template>
<style scoped>
/* Styles for the weather widget container */
.weather-widget {
  background: #f0f8ff;
  border: 2px solid #69b3f0;
  border-radius: 6px;
  padding: 1em;
  font-weight: bold;
  color: #1b4965;
}
</style>