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
  background-color: var(--porcelain);
  border-right: 2px solid var(--muted-teal);
  overflow-y: auto;
  z-index: 10;
}
</style>
