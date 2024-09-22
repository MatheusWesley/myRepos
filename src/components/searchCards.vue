<!-- HTML -->
<template>
  <section class="filter">
    <div class="input-wrapper">
      <ion-icon name="search-outline"></ion-icon>
      <input type="text" placeholder="Buscar..." />
    </div>
    <button>
      <ion-icon name="filter-outline"></ion-icon>
      <span class="sr-only">Filtrar</span>
    </button>
  </section>
</template>

<!-- JavaScript -->
<script setup>
defineOptions({
  name: 'searchCards'
})
</script>

<!-- CSS -->
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

  .input-wrapper {
    padding: 0 0.8rem;
  }

  .input-wrapper input::placeholder {
    content: "Buscar...";
  }
}
</style>