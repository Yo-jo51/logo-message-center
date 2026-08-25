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
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 16px 20px;
  background-color: #fffdf9;
  border: 1px solid #87a687;
  border-left: 6px solid darkolivegreen;
  border-radius: 3px;
  text-align: left;
  cursor: pointer;
  box-sizing: border-box;
  box-shadow: 0 2px 4px rgba(79, 110, 79, 0.12);
  transition:
    background-color 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease;
}

.Card:hover {
  background-color: #f4f1ea;
  border-color: #4f6e4f;
  box-shadow: 0 4px 10px rgba(79, 110, 79, 0.2);
}

.Card.ifseen {
  background-color: #e6e2d8;
  border: 1px solid #c8c0ae;
  border-left: 6px solid #c8c0ae;
  box-shadow: none;
}

.Card.ifseen:hover {
  background-color: #dad5ca;
  border-color: #b8b09e;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sender {
  font-weight: 800;
  color: #2e442e;
  font-size: 15px;
}

.Card.ifseen .sender {
  font-weight: 500;
  color: #555555;
}

.date {
  font-weight: 600;
  font-size: 12px;
  color: #4f6e4f;
}

.Card.ifseen .date {
  font-weight: normal;
  color: #777777;
}

.subject {
  color: #111111;
  font-weight: 600;
  font-size: 14px;
  line-height: 1.4;
}

.Card.ifseen .subject {
  color: #666666;
  font-weight: normal;
}
</style>
