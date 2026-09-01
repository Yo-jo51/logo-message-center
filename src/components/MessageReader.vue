<script setup lang="ts">
defineProps<{
  message: {
    sender: string
    reciever: string | null
    timestamp: string
    subject: string
    body: string | null
  }
}>()
</script>

<template>
  <div class="reader" :class="{ 'empty-state': !message }">
    <div class="message" v-if="message">
      <p class="meta-line">From: {{ message.sender }}</p>
      <p class="meta-line">To: {{ message.reciever || 'you' }}</p>

      <p id="Date">Date: {{ message.timestamp }}</p>
      <h2 id="Subject">{{ message.subject }}</h2>
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
  display: flex;
  flex-direction: column;
}

.reader.empty-state {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

h3 {
  font-weight: 900;
  font-size: x-large;
  max-width: 45ch;
  color: #7a7a7a;
  transform: translateY(-20px);
}

#Subject {
  padding-top: 6px;
  border-top: 1px solid #c8c0ae;
}

#body {
  margin-top: 20px;
  max-width: 70ch;
}

#Date {
  margin-top: 6px;
  margin-bottom: 6px;
  color: rgb(100, 99, 99);
}

.meta-line {
  font-size: larger;
  margin: 2px 0;
}
</style>
