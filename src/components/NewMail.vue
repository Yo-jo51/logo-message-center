<script setup lang="ts">
import { ref, computed } from 'vue'

const recipient = ref('')
const subject = ref('')
const body = ref('') // Enthält jetzt den formatierten HTML-String
const priority = ref(false)

const hasAttemptedSubmit = ref(false)
const editorRef = ref<HTMLDivElement | null>(null)

const emit = defineEmits<{
  (
    e: 'send',
    payload: { recipient: string; subject: string; body: string; priority: boolean },
  ): void
}>()

const isEmailValid = computed(() => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(recipient.value.trim()))
const showError = computed(() => hasAttemptedSubmit.value && !isEmailValid.value)

// Funktion zum Formatieren des markierten Textes
const format = (command: string) => {
  document.execCommand(command, false, '')
  updateBody()
  editorRef.value?.focus()
}

// Aktualisiert die reactive Variable mit dem HTML-Inhalt des Editors
const updateBody = () => {
  if (editorRef.value) {
    body.value = editorRef.value.innerHTML
  }
}

const sendEmail = () => {
  hasAttemptedSubmit.value = true

  if (!isEmailValid.value) {
    return
  }

  emit('send', {
    recipient: recipient.value.trim(),
    subject: subject.value.trim(),
    body: body.value, // Sendet den formatierten HTML-Text
    priority: priority.value,
  })

  recipient.value = ''
  subject.value = ''
  body.value = ''
  priority.value = false

  if (editorRef.value) {
    editorRef.value.innerHTML = '' // Leert das Editor-Spielfeld
  }

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
        <label>Message</label>
        <div class="toolbar">
          <button type="button" @click="format('bold')"><b>B</b></button>
          <button type="button" @click="format('italic')"><i>I</i></button>
          <button type="button" @click="format('underline')"><u>U</u></button>
        </div>

        <div
          ref="editorRef"
          class="text-editor"
          contenteditable="true"
          @input="updateBody"
          placeholder="Write your message..."
        ></div>
      </div>

      <div class="buttons">
        <button class="send-btn" :class="{ 'btn-disabled': showError }" @click="sendEmail">
          Send
        </button>

        <label><input type="checkbox" v-model="priority" /> High Priority</label>
      </div>
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
.text-editor {
  width: 100%;
  padding: 10px;
  border: 1px solid #c8c0ae;
  border-radius: 0 0 4px 4px;
  background-color: #fffdf9;
  font: inherit;
  color: #111;
  box-sizing: border-box;
  transition:
    border-color 0.2s ease,
    box-shadow 0.2s ease;
}

input {
  border-radius: 4px;
}

input:focus,
.text-editor:focus {
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

.toolbar {
  display: flex;
  gap: 4px;
}

.toolbar button {
  background: #fffdf9;
  border: 1px solid #c8c0ae;
  border-radius: 3px;
  padding: 4px 10px;
  cursor: pointer;
  font-family: inherit;
  font-size: 0.9rem;
}

.toolbar button:hover {
  background-color: #e2dacb;
}

.text-editor {
  min-height: 250px;
  max-height: 300px;
  overflow-y: auto;
  text-align: left;
  word-break: break-word;
}

.text-editor:empty:before {
  content: attr(placeholder);
  color: #a09885;
  pointer-events: none;
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

.buttons {
  display: flex;
  justify-content: start;
  align-items: center;
  margin-top: 20px;
  gap: 20px;
  white-space: nowrap;
}

.buttons label {
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
}
</style>
