<script setup></script>

<template>
  <div class="messagelist"></div>
</template>

<style scoped>
.messagelist {
  position: fixed;
  top: 20vh;
  left: 0;
  width: 350px;
  height: 80vh;
  background-color: var(--color-surface);
  border-right: 2px solid var(--color-border-dark);
  overflow-y: auto;
  z-index: 10;
}
</style>
