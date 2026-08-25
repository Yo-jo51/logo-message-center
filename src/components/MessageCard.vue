<script setup lang="ts">
const props = defineProps<{
  message: {
    sender: string
    subject: string
    timestamp: string
  }
}>()

const emit = defineEmits(['openMail'])
</script>

<template>
  <button class="Card" @click="emit('openMail', props.message)">
    <span class="sender">
      {{ props.message?.sender || 'Unbekannter Sender' }}
      <span class="date">- {{ props.message?.timestamp }}</span>
    </span>

    <span class="subject">{{ props.message?.subject || 'Kein Betreff' }}</span>
  </button>
</template>

<style scoped>
.Card {
  box-sizing: border-box;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 16px 20px;
  background: #fffdf9;
  border: 1px solid #87a687;
  border-left: 6px solid darkolivegreen;
  border-radius: 3px;
  text-align: left;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(79, 110, 79, 0.12);
  transition: 0.2s ease;
}

.Card:hover {
  background: #f4f1ea;
}

.Card.ifseen {
  background: #e6e2d8;
  border-color: #c8c0ae;
  border-left-color: #c8c0ae;
  box-shadow: none;
}

.Card.important {
  background: #fffaa0;
  border-left-color: #eb9c5c;
}

.Card.selected {
  background: #d1e7dd;
  border-color: #1f4e1f;
  box-shadow: 0 0 0 2px #4f6e4f;
}

.Card.ifseen .sender {
  font-weight: 500;
  color: #555;
}

.Card.ifseen .date {
  font-weight: normal;
  color: #777;
}

.Card.ifseen .subject {
  font-weight: normal;
  color: #666;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sender {
  font-size: 15px;
  font-weight: 800;
  color: #2e442e;
}

.date {
  font-size: 12px;
  font-weight: 600;
  color: #4f6e4f;
}

.subject {
  font-size: 14px;
  font-weight: 600;
  line-height: 1.4;
  color: #111;
}
</style>
