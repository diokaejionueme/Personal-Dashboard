<script setup>
// NewsWidget.vue - Fetches and displays top news headlines
import { ref, onMounted } from 'vue';

// State variables
const articles = ref([])      // List of news articles
const loading = ref(true)     // Loading indicator
const error    = ref(null)    // Error message

// Fetch news headlines from NewsAPI on component mount
onMounted(async () => {
  const apiKey = import.meta.env.VITE_NEWS_API_KEY // API key from .env
  const url = `https://newsapi.org/v2/top-headlines?country=us&pageSize=5&apiKey=${apiKey}`
  try {
    const res = await fetch(url, {
        headers: {
            'User-Agent': 'Mozilla/5.0',
        }
    })
    if (!res.ok) throw new Error(`${res.status} ${res.statusText}`)
    const data = await res.json()
    articles.value = data.articles // Store articles
  } catch (err) {
    error.value = err.message || 'Failed to fetch News' // Handle errors
  } finally {
    loading.value = false // Hide loading indicator
  }
})
</script>

<template>
  <!-- News widget UI: shows loading, error, or news list -->
  <div class="news-widget">
    <div v-if="loading">Loading news...</div>
    <div v-else-if="error"> ⚠️{{ error }}</div>
    <ul v-else>
      <li v-for="(article, i) in articles" :key="i">
        <a :href="article.url" target="_blank" rel="noopneer">
          {{ article.title }}
        </a>
        <div class="source">{{ article.source.name }}</div>
      </li>
    </ul>
  </div>
</template>

<style scoped>

/* Styles for the news widget container and list */
.news-widget {
  background: #faf3dd;
  border: 2px solid #f08a5d;
  border-radius: 6px;
  padding: 1em;
  font-weight: bold;
  color: #53354a;
}
.news-widget ul {
  list-style: none;
  padding: 0;
  margin: 0.5em 0 0;
}
.news-widget li {
  margin-bottom: 0.75em;
}
.news-widget li:last-child {
  margin-bottom: 0;
}
.news-widget a {
  color: #f08a5d;
  text-decoration: none;
}
.news-widget a:hover {
  text-decoration: underline;
}
.news-widget .source {
  font-size: 0.85em;
  color: #999;
}

</style>