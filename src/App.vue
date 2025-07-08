<script setup>
// Import the reactive function from Vue to make objects reactive
import { reactive } from 'vue';
import NoteWidget from './components/NoteWidget.vue';

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
    <header style="grid-area: header;">My Dashboard</header>
    <!-- Sidebar section (can add navigation or links here) -->
    <aside style="grid-area: sidebar;">Sidebar

    </aside>
  
   
    <!-- Main content area where widgets are displayed -->
    <main style="grid-area: main;">
      Main Content Area
      <!-- Loop through each widget and display it with a toggle button -->
      <div v-for="item in widgets" :key="item" class="widget-row">
        <!-- Show the widget only if its visibility is true -->
        <div class="widget">
          <!--render NoteWidget when item is "Notes"-->
          <NoteWidget 
           v-show="item === 'Notes' && visibility[item]"
          />
          <!--Fallback placeholder for all other widgets-->
          <div v-show="visibility[item]">  
            {{ item }} Widget
          </div>
        </div>
        <!-- Button to toggle the widget's visibility on double-click -->
        <button @click="toggle(item)">
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
    background: #555;   /* dark grey background */
    color: #fff;        /* white text for readability */
    padding: 1em;

  }
  main{
    grid-area: main;
    background: #f9f9f9;    /* very light gray for a bit of contrast */
    color: #333;             /* dark text for readability */
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
</style>