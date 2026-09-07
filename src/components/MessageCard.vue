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
  background: var(--color-surface);
  border: 1px solid var(--color-border-dark);
  border-left: 6px solid var(--color-primary);
  border-radius: 3px;
  text-align: left;
  cursor: pointer;
  box-shadow: 0 2px 4px rgb(37 99 235 / 0.12);
  transition: 0.2s ease;
}

.Card:hover {
  background: var(--color-surface-hover);
}

.Card.seen {
  background: var(--color-surface-hover);
  border-color: var(--color-border);
  border-left-color: var(--color-border);
  box-shadow: none;
}

.Card.important {
  background: var(--color-priority-muted);
  border-left-color: var(--color-priority);
}

.Card.selected {
  background: var(--color-unread);
  border: 2px solid var(--color-navy);
  border-left: 6px solid var(--color-navy);
  box-shadow: none;
}

.Card.seen .sender {
  font-weight: 500;
  color: var(--color-text-secondary);
}

.Card.seen .date {
  font-weight: normal;
  color: var(--color-text-muted);
}

.Card.seen .subject {
  font-weight: normal;
  color: var(--color-text-secondary);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sender {
  font-size: 15px;
  font-weight: 800;
  color: var(--color-text);
}

.date {
  font-size: 12px;
  font-weight: 600;
  color: var(--color-primary);
}

.subject {
  font-size: 14px;
  font-weight: 600;
  line-height: 1.4;
  color: var(--color-text);
}
</style>
