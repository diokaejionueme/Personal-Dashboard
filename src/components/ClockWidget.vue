<script setup>
// Import Vue composition API functions
import { ref, onMounted, onUnmounted } from 'vue'

// Reactive variable to hold the current time as a string
const time = ref('')
let timer // Timer ID for setInterval

// Updates the time variable with the current local time
function updateTime() {
  const now = new Date()
  time.value = now.toLocaleTimeString()
}

// Start the timer when the component is mounted
onMounted(() => {
  updateTime() // Set initial time
  timer = setInterval(updateTime, 1000) // Update every second
})

// Clear the timer when the component is unmounted
onUnmounted(() => clearInterval(timer))
</script>


<template>
  <!-- Clock widget UI displaying the current time -->
  <div class="clock-widget">
    ⏰ Current Time: {{ time }}
  </div>
</template>


<style scoped>
/* Styles for the clock widget container */
.clock-widget {
  background: #eef6fd;
  border: 2px solid #4a90e2;
  border-radius: 6px;
  padding: 1em;
  color: #2a3d66;
  font-weight: bold;
}
/* Alternative style (commented out)
.clock-widget {
  padding: 1em;
  border: 1px solid #cccc;
  background: white;
  font-size: 1.2em;
}
*/
</style>