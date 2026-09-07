<script setup>
import { ref } from 'vue'

const emit = defineEmits(['FilterApplied'])

const messageFilter = ref('All Messages')
const sortFilter = ref('Newest')

function FilterChange() {
  emit('FilterApplied', {
    status: messageFilter.value,
    sort: sortFilter.value,
  })
}

defineProps(['totalCount', 'unreadCount', 'readCount', 'prioritycount'])
</script>

<template>
  <div class="filters-container">
    <div class="filters">
      <select v-model="messageFilter" @change="FilterChange" class="filter-select">
        <option value="All Messages">All Messages ({{ totalCount }})</option>

        <option value="Unseen">Unseen ({{ unreadCount }})</option>

        <option value="Seen">Seen ({{ readCount }})</option>

        <option value="Priority">High Priority ({{ prioritycount }})</option>
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
  border-right: 1px solid var(--color-border-dark);
}

.filter-select {
  width: 50%;
  flex: 1;
  height: 40px;
  padding: 0 10px;
  background: var(--color-surface);
  border: none;
  border-bottom: 1px solid var(--color-border-dark);
  font-size: 14px;
  margin-top: 1px;
}
</style>
