<script setup>
// —— Dark‑mode toggle logic ——
import { ref, onMounted, watch, reactive } from 'vue'

// State for dark mode
const isDark = ref(false)
// Toggle dark mode state
function toggleDark() {
  isDark.value = !isDark.value
}

// Watch for changes and update document class and localStorage
watch(isDark, val => {
  document.documentElement.classList.toggle('dark', val)
  localStorage.setItem('theme', val ? 'dark' : 'light')
})

// On mount, restore theme from localStorage
onMounted(() => {
  const saved = localStorage.getItem('theme')
  if (saved === 'dark') {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
})
// —— End dark‑mode logic ——

// —— Widget import and visibility logic ——
import NoteWidget from './components/NoteWidget.vue'
import ClockWidget from './components/ClockWidget.vue'
import WeatherWidget from './components/WeatherWidget.vue'
import NewsWidget from './components/NewsWidget.vue'

// List of widget names
const widgets = ['Weather', 'Notes', 'News', 'Clock']
// Track which widgets are visible
const visibility = reactive({})
widgets.forEach(item => visibility[item] = true)
// Toggle widget visibility
const toggle = item => visibility[item] = !visibility[item]
</script>

<template>
  <!-- Main dashboard layout -->
  <div class="dashboard">
    <header>Dioka's Dashboard</header>

    <aside>
      <h2>Tools</h2>
      <!-- Dark Mode Toggle Button -->
      <button @click="toggleDark" style="margin-bottom:1em;">
        {{ isDark ? '🌞 Light Mode' : '🌙 Dark Mode' }}
      </button>

      <!-- Widget visibility controls -->
      <ul style="list-style:none; padding:0; margin:0;">
        <li v-for="item in widgets" :key="item" style="margin-bottom:0.5em;">
          <button @click="toggle(item)" style="width:100%;">
            {{ visibility[item] ? `Hide ${item}` : `Show ${item}` }}
          </button>
        </li>
      </ul>
    </aside>

    <main>
      <!-- Render each widget if visible -->
      <div v-for="item in widgets" :key="item" class="widget-row">
        <div class="widget">
          <NoteWidget    v-show="item === 'Notes'   && visibility[item]" />
          <ClockWidget   v-show="item === 'Clock'   && visibility[item]" />
          <WeatherWidget v-show="item === 'Weather' && visibility[item]" />
          <NewsWidget    v-show="item === 'News'    && visibility[item]" />

          <!-- fallback for unknown widgets -->
          <div v-show="visibility[item] && !['Notes','Clock','Weather','News'].includes(item)">
            {{ item }} Widget
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.dashboard {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: 60px 1fr;
  grid-template-areas:
    "header header"
    "sidebar main";
  height: 100vh;
}

header {
  grid-area: header;
  background: #34495e;
  color: white;
  padding: 1em;
}

aside {
  grid-area: sidebar;
  background: #555;
  color: #fff;
  padding: 1em;
  display: flex;
  flex-direction: column;
}

main {
  grid-area: main;
  background: var(--bg);
  color: var(--fg);
  padding: 1em;
  overflow-y: auto;
}

.widget-row {
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}

.widget {
  margin-right: 1em;
}

/* Style for sidebar buttons, can override with CSS variables */
aside button {
  background-color: var(--button-bg);
  color: var(--fg);
  border: 1px solid transparent;
  padding: 0.5em;
  border-radius: 4px;
  cursor: pointer;
  transition: border-color 0.2s;
}
aside button:hover {
  border-color: var(--button-border-hover);
}
</style>
