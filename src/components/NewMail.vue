<script setup lang="ts">
import { ref, computed } from 'vue'

const recipient = ref('')
const subject = ref('')
const body = ref('')

const hasAttemptedSubmit = ref(false)

const emit = defineEmits<{
  (e: 'send', payload: { recipient: string; subject: string; body: string }): void
}>()

const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

const isEmailValid = computed(() => emailRegex.test(recipient.value.trim()))
const showError = computed(() => hasAttemptedSubmit.value && !isEmailValid.value)

const sendEmail = () => {
  hasAttemptedSubmit.value = true

  if (!isEmailValid.value) {
    return
  }

  emit('send', {
    recipient: recipient.value.trim(),
    subject: subject.value.trim(),
    body: body.value,
  })

  recipient.value = ''
  subject.value = ''
  body.value = ''
  hasAttemptedSubmit.value = false
}
</script>

<template>
  <div class="reader">
    <div class="new-mail">
      <h2>New Message</h2>

      <div class="input-group">
        <label for="recipient">To</label>
        <input
          id="recipient"
          v-model="recipient"
          type="text"
          placeholder="Recipient..."
          :class="{ 'input-error': showError }"
        />
        <p v-if="showError" class="error-text">Please enter a valid email address.</p>
      </div>

      <div class="input-group">
        <label for="subject">Subject</label>
        <input id="subject" v-model="subject" type="text" placeholder="Subject..." />
      </div>

      <div class="input-group">
        <label for="body">Message</label>
        <textarea id="body" v-model="body" placeholder="Write your message..."></textarea>
      </div>

      <button class="send-btn" :class="{ 'btn-disabled': showError }" @click="sendEmail">
        Send
      </button>
    </div>
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

.new-mail {
  width: 100%;
  max-width: 900px;
}

h2 {
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-bottom: 15px;
}

label {
  font-weight: 700;
}

input,
textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #c8c0ae;
  border-radius: 4px;
  background-color: #fffdf9;
  font: inherit;
  color: #111;
  box-sizing: border-box;
  transition:
    border-color 0.2s ease,
    box-shadow 0.2s ease;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: darkolivegreen;
}

input.input-error {
  border-color: #b93a3a;
  background-color: #fff9f9;
}

input.input-error:focus {
  box-shadow: 0 0 0 2px rgba(185, 58, 58, 0.2);
}

.error-text {
  color: #b93a3a;
  font-size: 0.85rem;
  margin: 0;
  font-weight: 600;
}

textarea {
  min-height: 250px;
  resize: vertical;
}

.send-btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: darkolivegreen;
  color: white;
  font-weight: 700;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.send-btn:hover {
  background-color: #506f3a;
}

.send-btn.btn-disabled {
  background-color: #8c9c84;
  cursor: not-allowed;
}
</style>
