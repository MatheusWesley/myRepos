<!-- HTML -->
<template>
  <section class="kanban">
    <div v-for="section in sectionCards" :key="section.id" :class="section.classSecionCard">
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
</template>

<!-- JavaScript -->
<script setup>
import { ref } from 'vue';

defineOptions({
  name: 'showCards'
})

const sectionCards = ref([
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
      },
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
        tags: ['rocketseat', 'desafio'],
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
])
</script>

<!-- CSS -->
<style scoped>
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