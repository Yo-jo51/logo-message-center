<script setup>
import { ref } from 'vue'

const emit = defineEmits(['FilterApplied'])

const messageFilter = ref('All Messages')
const sortFilter = ref('Oldest')

function FilterChange() {
  emit('FilterApplied', {
    status: messageFilter.value,
    sort: sortFilter.value,
  })
}
</script>

<template>
  <div class="filters-container">
    <div class="filters">
      <select v-model="messageFilter" @change="FilterChange" class="filter-select">
        <option value="All Messages">All Messages</option>
        <option value="Unseen">Unseen</option>
        <option value="Seen">Seen</option>
      </select>

      <select v-model="sortFilter" @change="FilterChange" class="filter-select">
        <option value="Newest">Newest</option>
        <option value="Oldest">Oldest</option>
      </select>
    </div>
  </div>
</template>

<style scoped>
.filters-container {
  width: 100%;
  flex-shrink: 0;
}

.filters {
  display: flex;
  width: 100%;
  margin-top: 42px;
  border-right: 1px solid var(--muted-teal);
}

.filter-select {
  width: 50%;
  flex: 1;

  height: 40px;
  padding: 0 10px;

  background: var(--porcelain);
  border: none;
  border-bottom: 1px solid var(--muted-teal);

  font-size: 14px;

  margin-top: 1px;
}
</style>
