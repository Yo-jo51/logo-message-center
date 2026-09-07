<script setup>
import { ref } from 'vue'

const search = ref('')
const emit = defineEmits(['SearchChanged'])

function searchChanged() {
  emit('SearchChanged', search.value)
}
</script>

<template>
  <div class="search-container">
    <input
      v-model="search"
      @input="searchChanged"
      type="text"
      placeholder="Search Messages..."
      class="search"
    />
  </div>
</template>

<style scoped>
.search-container {
  position: fixed;
  top: 15vh;
  left: 0;
  width: 350px;
  z-index: 10;
  background: var(--color-surface);
}

.search {
  width: 100%;
  height: 5vh;
  padding-left: 10px;
  box-sizing: border-box;
  border: 1px solid var(--color-border-dark);
  border-radius: 0;
  background: transparent;
  outline: none;
}
</style>
