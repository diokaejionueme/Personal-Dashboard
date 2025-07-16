<script setup>
import { ref, computed, onMounted } from 'vue'

const weather = ref(null)
const loading = ref(true)
const error   = ref(null)

const iconUrl = computed(() => {
  if (!weather.value) return ''
  return `https://openweathermap.org/img/wn/${weather.value.weather[0].icon}@2x.png`
})

onMounted(async () => {
  const apiKey = import.meta.env.VITE_OPEN_WEATHER_API_KEY
  const city   = 'Abuja'

  try {
    const res = await fetch(
      `https://api.openweathermap.org/data/2.5/weather` +
      `?q=${city}&units=metric&appid=${apiKey}`
    )
    if (!res.ok) {
      throw new Error(`API error: ${res.status} ${res.statusText}`)
    }
    weather.value = await res.json()
  }
  catch (err) {
    error.value = err.message || 'Failed to fetch weather'
  }
  finally {
    loading.value = false
  }
})
</script>

<template>
  <div class="weather-widget">
    <div v-if="loading">Loading weather…</div>
    <div v-else-if="error">⚠️ {{ error }}</div>
    <div v-else class="weather-data">
      <img :src="iconUrl" alt="weather icon" />
      <div>
        <strong>{{ weather.name }}</strong>: 
        {{ weather.main.temp.toFixed(1) }}°C, 
        {{ weather.weather[0].description }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.weather-widget {
  background: #f0f8ff;
  border: 2px solid #69b3f0;
  border-radius: 6px;
  padding: 1em;
  font-weight: bold;
  color: #1b4965;
  display: flex;
  flex-direction: column;
}

.weather-data {
  display: flex;
  align-items: center;
  gap: 0.75em;
}

.weather-data img {
  width: 48px;
  height: 48px;
}
</style>