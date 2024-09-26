<template>
  <div>
    <button class="menu-toggle" @click="toggleMenu">
      <ion-icon :name="menuOpen ? 'close' : 'menu'"></ion-icon>
    </button>

    <nav :class="{ 'is-active': menuOpen }">
      <img
        src="../assets/github-mark-white.svg"
        alt="Logo do site"
        class="logo"
      />

      <ul>
        <li v-for="item in items" :key="item.id">
          <a :href="item.link" :class="item.class" :target="item.target">
            <ion-icon :name="item.icon"></ion-icon>
            {{ item.name }}
          </a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
import { ref } from "vue";

const menuOpen = ref(false);
const toggleMenu = () => {
  menuOpen.value = !menuOpen.value;
};

const items = ref([
  {
    id: 1,
    name: "Pagina Inicial",
    icon: "home",
    link: "#",
    class: "active",
  },
  {
    id: 2,
    name: "Github",
    icon: "logo-github",
    link: "https://github.com/matheuswesley",
    target: "_blank",
  },
  {
    id: 3,
    name: "Linkedin",
    icon: "logo-linkedin",
    link: "https://www.linkedin.com/in/mathwesley/",
  },
]);
</script>

<style scoped>
/* Estilos gerais */
a {
  text-decoration: none;
}

nav {
  padding: 3.2rem 2.7rem 0 3.5rem;
}

ul {
  margin-top: 6rem;
  list-style: none;
  display: grid;
  gap: 3.6rem;
  min-width: 11.5rem;
}

li a {
  color: #e0d7f7;
  display: flex;
  align-items: center;
  gap: 1.6rem;
  font-weight: 500;
}

a.active,
a:hover {
  color: white;
  font-weight: 700;
}

a ion-icon {
  font-size: 2rem;
}

/* Botão de menu para mobile */
.menu-toggle {
  display: none;
}

/* Estilos para mobile */
@media (max-width: 768px) {
  /* Botão de menu para mobile */
  .menu-toggle {
    display: inline;
    position: fixed;
    top: 20px;
    right: 20px;
    background: none;
    border: none;
    color: #2c2f33;
    font-size: 2.5rem;
    z-index: 10;
  }

  nav {
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    height: 100%;
    width: 250px;
    background-color: #2c2f33;
    transform: translateX(-100%);
    transition: transform 0.3s ease;
  }

  nav.is-active {
    transform: translateX(0);
  }

  ul {
    margin-top: 2rem;
    gap: 2rem;
  }
}
</style>
