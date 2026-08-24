<script setup>
// Hier kommt später die v-model Suchlogik rein
</script>

<template>
  <div class="search-container">
    <input type="text" placeholder="Search Messages..." class="search" />
  </div>
</template>

<style scoped>
.search-container {
  position: fixed;
  top: 15vh;
  left: 0;
  width: 350px;
  z-index: 10;
  background-color: var(--porcelain);
}

.search {
  width: 100%;
  height: 5vh;

  padding-left: 10px;

  box-sizing: border-box;

  border: 1px solid var(--muted-teal);

  border-radius: 0px;

  background: transparent;

  outline: none;
}
</style>
