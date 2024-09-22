<!-- KanbanWithSearch.vue -->
<template>
  <div class="kanban-with-search">
    <!-- Componente de busca -->
    <section class="filter">
      <div class="input-wrapper">
        <ion-icon name="search-outline"></ion-icon>
        <input type="text" placeholder="Buscar..." v-model="searchQuery" @input="filterCards" />
      </div>

      <!-- Filtro Dropdown -->
      <div class="filtered-dropdown">
        <button @click="toggleFilterMenu">
          <ion-icon name="filter-outline"></ion-icon>
          <span class="sr-only">Filtrar</span>
        </button>
        <!-- Menu de filtros (oculto por padrão) -->
        <ul v-if="showFilterMenu" class="filter-menu">
          <li v-for="tag in allTags" :key="tag">
            <input type="checkbox" v-model="selectedTags" :value="tag" @change="filterCards">
            {{ tag }}
          </li>
        </ul>
      </div>
    </section>


    <!-- Componente Kanban -->
    <section class="kanban">
      <div v-for="section in filteredSections" :key="section.id" :class="section.classSecionCard">
        <h2>{{ section.title }}</h2>
        <div class="cards">
          <div class="card" v-for="card in section.card" :key="card.name">
            <h3>{{ card.name }}</h3>
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

const searchQuery = ref('');
const selectedTags = ref([]);
const showFilterMenu = ref(false);

const originalSections = ref([
  {
    id: 1,
    title: 'A Fazer',
    classSecionCard: 'todo',
    card: [
      {
        name: '#boraCodar um Kanban 🧑🏾‍💻',
        description: 'Novo desafio do #boraCodar da Rocketseat, onde é proposto construir um quadro de Kanban.',
        tags: ['rocketseat', 'desafios'],
      },
      {
        name: 'Manter a ofensiva 🔥',
        description: 'Manter minha atividade na plataforma da Rocketseat para não perder a ofensiva',
        tags: ['rocketseat'],
      }
    ]
  },
  {
    id: 2,
    title: 'Fazendo',
    classSecionCard: 'doing',
    card: [
      {
        name: 'Conferir o novo desafio 🚀',
        description: 'Conferir o novo projeto do #boraCodar para fazê-lo da melhor maneira possível',
        tags: ['rocketseat', 'desafio', 'oi', 'javascript'],
      }
    ]
  },
  {
    id: 3,
    title: 'Feito',
    classSecionCard: 'done',
    card: [
      {
        name: '#boraCodar uma página de login 🧑‍💻',
        description: 'Novo desafio do #boraCodar da Rocketseat, onde é proposto construir uma página de login.',
        tags: ['rocketseat', 'desafio'],
      }
    ]
  }
]);

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

onMounted(() => {
  // Você pode adicionar qualquer lógica de inicialização aqui, se necessário
});
</script>

<style scoped>
/* Section Filter */
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

.filter-menu {
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  position: absolute;
  z-index: 1;
  right: 4rem;

  padding: 1rem;
  margin-top: .5rem;
  border: 0;
  background: #243236;
  border-radius: 0.5rem;
  color: white;

  font-size: 1.2rem;
  list-style-type: none;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
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

@media (max-width: 480px) {
  section.filter {
    flex-direction: row-reverse;
  }

  .filter-menu {
    position: absolute;
    z-index: 1;
    left: 4rem;

  }

  .input-wrapper {
    padding: 0 0.8rem;
  }

  .input-wrapper input::placeholder {
    content: "Buscar...";
  }
}

/* Section Kanban */
section.kanban {
  display: flex;
  flex-direction: column;
  gap: 2.4rem;
  padding: 1.6rem;
}

section.kanban>div {
  background: #e6e5eb;
  border-radius: 0.8rem;
  padding: 1.6rem;
}

section.kanban h2 {
  font-size: 1.8rem;
  color: #403937;
  padding: 1.2rem 0;
  border-bottom: 1px solid #e0e0e0;
  margin-bottom: 1.2rem;
}

.card {
  padding: 1.6rem;
  background: #ffffff;
  box-shadow: 0rem 0.2rem 0.8rem rgba(234, 226, 253, 0.5);
  border-radius: 0.8rem;
  margin-bottom: 1.6rem;
  line-height: 130%;
}

.card h3 {
  font-size: 1.4rem;
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

.cards {
  display: flex;
  flex-direction: column;
  gap: 1.6rem;
}

@media (min-width: 768px) {
  section.kanban {
    flex-direction: row;
    overflow-x: auto;
    padding: 2.4rem;
    gap: 2.4rem;
  }

  section.kanban>div {
    flex: 0 0 300px;
  }
}
</style>