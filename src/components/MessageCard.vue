<script setup lang="ts">
const props = defineProps<{
  message: {
    sender: string
    subject: string
  }
}>()

const emit = defineEmits(['openMail'])
</script>

<template>
  <button class="Card" @click="emit('openMail', props.message)">
    <span class="sender">{{ props.message?.sender || 'Unbekannter Sender' }}</span>
    <span class="subject">{{ props.message?.subject || 'Kein Betreff' }}</span>
  </button>
</template>

<style scoped>
.Card {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 4px;

  padding: 18px;
  background-color: #ffffff;
  border: 1px solid #b0bec5;

  text-align: left;
  cursor: pointer;
  box-sizing: border-box;
}

.sender {
  font-weight: bold;
  color: #333333;
}

.subject {
  color: #666666;
  font-size: 14px;
}

.Card:hover {
  background-color: #f5f5f5;
}
</style>
