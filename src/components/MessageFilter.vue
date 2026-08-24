<script setup></script>

<template>
  <div class="filters-container">
    <div class="filters">
      <select class="filter-select">
        <option>All Messages</option>
        <option>Unseen</option>
        <option>Seen</option>
      </select>
      <select class="filter-select">
        <option>Newest</option>
        <option>Oldest</option>
      </select>
    </div>
  </div>
</template>

<style scoped>
.filters-container {
  position: fixed;
  top: 20vh;
  left: 0;
  width: 350px;
  z-index: 10;
  background-color: var(--porcelain);
}

.filters {
  display: flex;
  border: none;
}

.filter-select {
  flex: 1;
  padding: 9px;

  border-right: 1px solid var(--muted-teal);
  border-bottom: 1px solid var(--muted-teal);
  border-top: none;
  border-left: none;

  background-color: var(--porcelain);
  width: 50%;
}
</style>
