<!-- HTML -->
<template>
  <div id="container">
    <mySidebar />
    <main>
      <div class="inner">
        <myHeader />
        <kanbanWithSearch />
      </div>
    </main>
  </div>
</template>

<!-- JavaScript -->
<script setup>
import myHeader from './components/myHeader.vue';
import mySidebar from './components/mySidebar.vue'
import kanbanWithSearch from './components/kanbanWithSearch.vue';

defineOptions({
  components: {
    mySidebar,
    myHeader,
    kanbanWithSearch
  }
})
</script>

<!-- CSS -->
<style scoped>
#container {
  display: grid;
  grid-template-columns: max-content 1fr;

  min-height: 100vh;
}

main .inner {
  height: 100%;
  background-color: #fbfaff;
  border-top-left-radius: 3.2rem;

  padding: 4.8rem 3.2rem;
}

main {
  padding-top: 0.8rem;
}
</style>
