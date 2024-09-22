<!-- HTML -->
<template>
  <section class="title-avatar">
    <h1>{{ data.title }}</h1>
    <img :src="data.avatar" alt="avatar" />
  </section>
</template>

<!-- JavaScript -->
<script setup>
import { ref } from 'vue';

defineOptions({
  name: 'myHeader',
})

const data = ref({
  title: 'Repositórios',
  avatar: 'https://github.com/matheuswesley.png'
})
</script>

<!-- CSS -->
<style scoped>
/* Section Title + Avatar */
section.title-avatar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  flex-wrap: wrap;
}

.title-avatar>img {
  width: 4.8rem;
  height: 4.8rem;
  border-radius: 50%;
}

h1 {
  font-size: 2.4rem;
  color: #403937;
  margin-right: 1rem;
}

/* Media query para dispositivos móveis */
@media (max-width: 768px) {
  section.title-avatar {
    flex-direction: row;
    gap: 1rem;
  }

  h1 {
    font-size: 2rem;
    align-items: flex-start;
    margin-right: 0;
  }

  .title-avatar>img {
    align-self: flex-end;
  }
}
</style>