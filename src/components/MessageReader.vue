<script setup lang="ts">
defineProps<{
  message: {
    sender: string
    subject: string
    body: string | null
  }
}>()
</script>

<template>
  <div class="reader">
    <div class="message" v-if="message">
      <p>Sender: {{ message.sender }}</p>
      <h2>{{ message.subject }}</h2>
      <p id="body">{{ message.body || 'No Content' }}</p>
    </div>

    <h3 v-else>
      No email selected yet. Please click on an email from the list to view it in the reader
    </h3>
  </div>
</template>

<style scoped>
.reader {
  flex: 1;
  height: 85vh;
  background-color: var(--parchment);
  padding: 20px;
  margin-top: 15vh;
  overflow-y: auto;
}
#body {
  margin-top: 20px;
  max-width: 70ch;
}
</style>
