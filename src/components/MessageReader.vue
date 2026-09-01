<script setup lang="ts">
interface Attachment {
  name: string
  path: string
  size: string
}

defineProps<{
  message: {
    sender: string
    reciever: string | null
    timestamp: string
    subject: string
    body: string | null
    attachments?: Attachment[]
  } | null
}>()
</script>

<template>
  <div class="reader" :class="{ 'empty-state': !message }">
    <div v-if="message" class="message">
      <p class="meta-line">From: {{ message.sender }}</p>
      <p class="meta-line">To: {{ message.reciever || 'you' }}</p>

      <p id="Date">Date: {{ message.timestamp }}</p>
      <h2 id="Subject">{{ message.subject }}</h2>

      <p id="body" v-html="message.body || 'No Content'"></p>

      <div class="attachments-section">
        <div class="attachments-grid">
          <a
            v-for="file in message.attachments || []"
            :key="file.path"
            :href="file.path"
            :download="file.name"
            class="attachment-chip"
          >
            <span class="file-name">📥 {{ file.name }}</span>

            <span class="file-size" style="font-size: 0.75rem; color: #666; margin-top: 2px">
              {{ file.size }}
            </span>
          </a>
        </div>
      </div>
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
  word-break: break-word;
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

.attachments-section {
  margin-top: 40px;
  padding-top: 15px;
  border-top: 1px dashed #c8c0ae;
  grid-row: column;
}

.attachments-section h4 {
  margin: 0 0 12px 0;
  color: #2f4f2f;
  font-size: 0.95rem;
  font-weight: 700;
}

.attachments-grid {
  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: flex-start;
}

.attachment-chip {
  display: flex;
  align-items: center;
  gap: 10px;
  background-color: #fffdf9;
  border: 1px solid #c8c0ae;
  border-radius: 6px;
  padding: 8px 14px;
  min-width: 180px;
  max-width: 280px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.03);
  text-decoration: none;
}

.attachment-chip:hover {
  background-color: #f7f3e9;
  border-color: darkolivegreen;
}

.file-name {
  font-size: 0.85rem;
  font-weight: 600;
  color: #111;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
