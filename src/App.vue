<template>
  <div class="app-container">
    <nav class="navbar">
      <h1 class="navbar-title">News Today</h1>
      <div class="category-buttons">
        <button v-for="category in categories" :key="category" @click="filterArticles(category)"
          class="category-button">{{ category }}
        </button>
      </div>
    </nav>
    <div class="main-content">
      <aside class="sidebar">
        <h2>MESSAGE</h2>
        <p>This is where you can add additional information, links, or widgets.</p>
        <p>More features coming soon!</p>
      </aside>
      <div class="news-list">
        <div v-if="loading" class="loader">Loading...</div>
        <div v-for="(article, index) in filteredArticles" :key="article.url" class="news-item">
          <img :src="article.urlToImage" alt="News image" class="news-image" />
          <h2 class="news-title">{{ article.title }}</h2>
          <p class="news-category">Category: {{ article.category }}</p>
          <p class="news-description">{{ article.description }}</p>
          <a :href="article.url" target="_blank" class="read-more">Read more</a>
        </div>
        <div v-if="!filteredArticles.length && !loading" class="no-articles">No articles available.</div>
      </div>
    </div>
    <footer class="footer">
      <p>Developed by Radhi Benafia with News API free</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      categories: ['business', 'entertainment', 'general', 'health', 'science', 'sports', 'technology'],
      articles: [],
      selectedCategory: null,
      loading: false // Nouvelle variable
    };
  },
  computed: {
    filteredArticles() {
      return this.selectedCategory
        ? this.articles.filter(article => article.category === this.selectedCategory)
        : this.articles;
    }
  },
  created() {
    this.fetchAllArticles();
  },
  methods: {
    async fetchAllArticles() {
      const apiKey = '0e06d93a0144497ab6570a86748e0f6b'; // Remplace par ta clé API
      const allArticles = [];
      this.loading = true; // Démarrer le chargement

      for (const category of this.categories) {
        try {
          const response = await fetch(`/api/v2/top-headlines?country=us&category=${category}&apiKey=${apiKey}`);
          if (!response.ok) throw new Error(`Error: ${response.status}`);

          const data = await response.json();

          if (data.articles && Array.isArray(data.articles)) {
            const articlesWithCategory = data.articles.slice(0, 10).map(article => ({
              ...article,
              category // Inclure la catégorie
            }));
            allArticles.push(...articlesWithCategory);
          } else {
            console.warn('No articles found for category:', category);
          }
        } catch (error) {
          console.error(`Failed to fetch articles for ${category}:`, error);
        }
      }

      this.articles = allArticles;
      this.loading = false; // Arrêter le chargement
    },
    filterArticles(category) {
      this.selectedCategory = category; // Définir la catégorie sélectionnée
    }
  }
};
</script>

<style scoped>
body {
  background: linear-gradient(to bottom, #e2e2e2, #ffffff);
  font-family: 'Arial', sans-serif;
}

.app-container {
  max-width: 1200px;
  margin: 30px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
}

.navbar {
  background-color: #007bff;
  padding: 20px;
  border-radius: 5px 5px 0 0;
  text-align: center;
}

.navbar-title {
  color: white;
  font-size: 36px;
  margin: 0;
}

.category-buttons {
  margin-top: 10px;
}

.category-button {
  margin: 0 5px;
  padding: 10px 15px;
  background-color: white;
  border: 1px solid #007bff;
  border-radius: 5px;
  color: #007bff;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
}

.category-button:hover {
  background-color: #e7f0ff;
  transform: scale(1.05);
}

.main-content {
  display: flex;
}

.sidebar {
  flex: 1;
  padding: 20px;
  background-color: #f1f1f1;
  border-right: 1px solid #ced4da;
  height: fit-content;
}

.news-list {
  flex: 3;
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 20px;
}

.loader {
  text-align: center;
  font-size: 18px;
  color: #007bff;
}

.news-item {
  padding: 15px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  background-color: #f8f9fa;
  transition: box-shadow 0.3s, transform 0.3s;
}

.news-item:hover {
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  transform: translateY(-2px);
}

.news-image {
  max-width: 100%;
  height: auto;
  border-radius: 5px;
}

.news-title {
  font-size: 24px;
  margin: 10px 0 5px;
  color: #007bff;
}

.news-category {
  font-size: 14px;
  color: #6c757d;
  margin: 5px 0;
}

.news-description {
  font-size: 16px;
  color: #343a40;
}

.read-more {
  display: inline-block;
  margin-top: 10px;
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  text-decoration: none;
  transition: background-color 0.3s, transform 0.3s;
}

.read-more:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.no-articles {
  color: #dc3545;
  font-size: 18px;
  text-align: center;
  margin-top: 20px;
}

.footer {
  margin-top: 30px;
  text-align: center;
  font-size: 14px;
  color: #343a40;
}
</style>
