<script setup>
// Import the reactive function from Vue to make objects reactive
import { reactive } from 'vue';

// List of widget names to display on the dashboard
const widgets = ['Weather', 'Notes', 'News', 'Clock']

// Create a reactive object to track the visibility of each widget
const visibility = reactive({})

// Set the initial visibility of each widget to true (visible)
widgets.forEach(item => visibility[item] = true)

// Function to toggle the visibility of a widget when called
const toggle = (item) => visibility[item] = !visibility[item]
</script>

<template>
  <div class="dashboard">
    <!-- Header section of the dashboard -->
    <header>My Dashboard</header>
    <!-- Sidebar section (can add navigation or links here) -->
    <aside>Sidebar</aside>
    <!-- Main content area where widgets are displayed -->
    <main>
      Main Content Area
      <!-- Loop through each widget and display it with a toggle button -->
      <div v-for="item in widgets" :key="item" class="widget-row">
        <!-- Show the widget only if its visibility is true -->
        <div v-show="visibility[item]" class="widget">
          {{ item }}
        </div>
        <!-- Button to toggle the widget's visibility on double-click -->
        <button @dblclick="toggle(item)">
          {{ visibility[item] ? 'Hide' : 'Show' }}
        </button>
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
    background: #ecf0f1;
    padding: 1em;
  }
  main {
    grid-area: main;
    padding: 1em;
  }

  .widget-row {
    display: flex;
    align-items: center;
    margin-bottom: 0.5em;
  }

  .widget {
    margin-right: 1em;
  }
</style>