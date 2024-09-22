<template>
  <div class="kanban-with-search">
    <!-- Componente de busca -->
    <section class="filter">
      <div class="input-wrapper">
        <ion-icon name="search-outline"></ion-icon>
        <input type="text" placeholder="Buscar por repositórios..." v-model="searchQuery" @input="filterCards" />
      </div>

      <!-- Filtro Dropdown -->
      <div class="filtered-dropdown">
        <button @click="toggleFilterMenu">
          <ion-icon name="filter-outline"></ion-icon>
        </button>
        <!-- Menu de filtros (oculto por padrão) -->
        <ul v-if="showFilterMenu" class="filter-menu">
          <li class="filter-title">Tags</li>
          <li v-for="tag in allTags" :key="tag" class="filter-item">
            <label>
              <input type="checkbox" v-model="selectedTags" :value="tag" @change="filterCards">
              <span class="checkmark"></span>
              {{ tag }}
            </label>
          </li>
        </ul>
      </div>
    </section>

    <!-- Componente Kanban -->
    <section class="kanban">
      <div v-for="section in filteredSections" :key="section.id" :class="section.classSectionCard">
        <h2>{{ section.title }} </h2>
        <div class="cards">
          <div class="card" v-for="card in section.card" :key="card.name">
            <h3>
              {{ card.name }}
              <a :href="card.link" target="_blank">
                <ion-icon name="link-outline"></ion-icon>
              </a>
            </h3>
            <p>{{ card.description }}</p>
            <div class="tags">
              <span v-for="tag in card.tags" :key="tag">{{ tag }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';

const searchQuery = ref('');
const selectedTags = ref([]);
const showFilterMenu = ref(false);
const originalSections = ref([]);
const token = import.meta.env.GITHUB_TOKEN;

// Função para buscar os repositórios do GitHub
async function fetchGitHubRepos(username) {
  try {
    const response = await axios.get(`https://api.github.com/users/${username}/repos`, {
      headers: {
        Authorization: `token ${token}`
      }
    });

    const repos = response.data.map(repo => ({
      name: repo.name,
      link: repo.html_url,
      description: repo.description || 'Sem descrição',
      tags: [],
      languages_url: repo.languages_url
    }));

    // Obter a contagem de repositórios públicos
    const userResponse = await axios.get(`https://api.github.com/users/${username}`, {
      headers: {
        Authorization: `token ${token}`
      }
    });
    const publicRepoCount = userResponse.data.public_repos;

    // Adiciona os repositórios à seção de "Repositórios GitHub" com a contagem
    originalSections.value = [
      {
        id: 1,
        title: `Você tem ${publicRepoCount} repositórios públicos`,
        classSectionCard: 'done',
        card: repos
      }
    ];

    // Usando for...of para iterar sobre os repositórios e buscar as linguagens
    for (const repo of repos) {
      const languagesResponse = await axios.get(repo.languages_url, {
        headers: {
          Authorization: `token ${token}`
        }
      });
      repo.tags = Object.keys(languagesResponse.data);
    }

  } catch (error) {
    console.error('Erro ao buscar repositórios:', error);
  }
}

onMounted(() => {
  fetchGitHubRepos('matheuswesley');
});

const allTags = computed(() => {
  const tags = new Set();
  for (const section of originalSections.value) {
    for (const card of section.card) {
      for (const tag of card.tags) {
        tags.add(tag);
      }
    }
  }
  return Array.from(tags);
});

const filteredSections = computed(() => {
  return originalSections.value.map(section => ({
    ...section,
    card: section.card.filter(card => {
      const matchesSearch = card.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        card.description.toLowerCase().includes(searchQuery.value.toLowerCase());
      const matchesTags = selectedTags.value.length === 0 ||
        card.tags.some(tag => selectedTags.value.includes(tag));
      return matchesSearch && matchesTags;
    })
  })).filter(section => section.card.length > 0);
});

const filterCards = () => {
  // A filtragem é feita automaticamente pelo computed property
};

const toggleFilterMenu = () => {
  showFilterMenu.value = !showFilterMenu.value;
};
</script>

<style scoped>
/* Seção Filter */
section.filter {
  margin-top: 1.6rem;
  padding: 0 1.2rem;
  display: flex;
  gap: 0.8rem;
}

.filter button {
  padding: 1rem;
  border: 0;
  background: #243236;
  border-radius: 0.5rem;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

.filter button ion-icon {
  font-size: 2rem;
}

.filtered-dropdown {
  position: relative;
}

.filter-menu {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  position: absolute;
  z-index: 1;
  right: 0;
  top: 100%;
  padding: 0.5rem;
  margin-top: 0.5rem;
  background: #243236;
  border-radius: 0.5rem;
  color: white;
  font-size: 1.2rem;
  list-style-type: none;
  min-width: 20rem;
  box-shadow: 0 .2rem 1rem rgba(0, 0, 0, 0.2);
}

.filter-title {
  font-weight: bold;
  padding: 0.5rem;
  border-bottom: 0.1rem solid rgba(255, 255, 255, 0.1);
  width: 100%;
  margin-bottom: 0.5rem;
}

.filter-item {
  padding: 0.5rem;
  width: 100%;
}

.filter-item label {
  display: flex;
  align-items: center;
  cursor: pointer;
  text-transform: capitalize;
}

.filter-item input[type="checkbox"] {
  display: none;
}

.checkmark {
  width: 2rem;
  height: 2rem;
  background-color: #ffffff;
  border-radius: 25%;
  display: inline-block;
  position: relative;
  margin-right: 1rem;
}

.filter-item input[type="checkbox"]:checked+.checkmark:after {
  content: '';
  position: absolute;
  left: 0.6rem;
  top: 0.2rem;
  width: 0.6rem;
  height: 1.2rem;
  border: solid black;
  border-width: 0 0.2rem 0.2rem 0;
  transform: rotate(45deg);
}

.input-wrapper {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  background: #ffffff;
  border: 0.07rem solid #e3e3e3;
  box-shadow: 0rem 0.2rem 0.8rem rgba(22, 22, 22, 0.1);
  border-radius: 0.8rem;
  padding: 0 1.2rem;
  flex: 1;
}

.input-wrapper ion-icon {
  font-size: 2rem;
  color: #505059;
}

.input-wrapper input {
  font-size: 1.4rem;
  border: 0;
  width: 100%;
  outline: 0;
  padding: 0.8rem 0;
}

.input-wrapper:has(:focus) {
  outline: 0.2rem black solid;
}

.input-wrapper input::placeholder {
  font-size: 1.4rem;
  color: #7c7c8a;
}

/* Seção Kanban */
section.kanban {
  display: flex;
  flex-wrap: wrap;
  gap: 2.4rem;
  padding: 1.6rem;
}

.cards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 1.6rem;
}

section.kanban>div {
  border-radius: 0.8rem;
  padding: 1.6rem;
  flex: 1 1 100%;
  box-sizing: border-box;
  transition: flex-grow 0.2s ease;
}

section.kanban h2 {
  font-size: 1.8rem;
  color: #403937;
  padding: 1.2rem 0;
  border-bottom: 0.1rem solid #e0e0e0;
  margin-bottom: 1.2rem;
}

.card {
  padding: 1.6rem;
  background: #ffffff;
  box-shadow: 0rem 0.2rem 0.8rem rgba(177, 171, 192, 0.5);
  border-radius: 0.8rem;
  line-height: 130%;
  flex: 1 1 calc(25% - 1.6rem);
  /* Alterado para 25% para 4 cards por linha */
  min-width: 200px;
  /* Reduzido para permitir 4 cards */
  max-width: calc(25% - 1.6rem);
  /* Alterado para 25% para 4 cards por linha */
}

.card h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1.4rem;
  text-transform: capitalize;
  color: #403937;
  margin-bottom: 0.8rem;
}

.card p {
  font-weight: 500;
  font-size: 1.3rem;
  color: #756966;
  margin-bottom: 1rem;
}

.card .tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.8rem;
}

.card .tags span {
  font-size: 1.1rem;
  line-height: 130%;
  background: #b3dae6;
  border-radius: 0.8rem;
  padding: 0.4rem 0.8rem;
  color: #243236;
  font-weight: 500;
}

/* Estilos responsivos */

@media (max-width: 1200px) {
  .card {
    flex: 1 1 calc(50% - 1.6rem);
    max-width: calc(50% - 1.6rem);
  }
}

@media (max-width: 768px) {
  section.kanban>div {
    flex: 1 1 100%;
  }
}
</style>