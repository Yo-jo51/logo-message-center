<script setup lang="ts">
import { ref, computed } from 'vue'

const recipient = ref('')
const subject = ref('')
const body = ref('')
const priority = ref(false)

const selectedFile = ref<File | null>(null)
const fileInput = ref<HTMLInputElement | null>(null)
const editorRef = ref<HTMLDivElement | null>(null)
const hasAttemptedSubmit = ref(false)

const emit = defineEmits<{
  (
    e: 'send',
    mail: {
      recipient: string
      subject: string
      body: string
      priority: boolean
      attachments: File[]
      fileName: string | null
    },
  ): void
}>()

const isEmailValid = computed(() => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(recipient.value.trim()))
const showError = computed(() => hasAttemptedSubmit.value && !isEmailValid.value)

function updateBody() {
  body.value = editorRef.value?.innerHTML || ''
}

function format(command: string) {
  document.execCommand(command)
  updateBody()
  editorRef.value?.focus()
}

function uploadFile(event: Event) {
  const input = event.target as HTMLInputElement
  selectedFile.value = input.files?.[0] || null
}

function sendEmail() {
  hasAttemptedSubmit.value = true
  if (!isEmailValid.value) return

  emit('send', {
    recipient: recipient.value.trim(),
    subject: subject.value.trim(),
    body: body.value,
    priority: priority.value,
    attachments: selectedFile.value ? [selectedFile.value] : [],
    fileName: selectedFile.value ? selectedFile.value.name : null, // Hier wird der Name übergeben
  })

  // Reset Form
  recipient.value = ''
  subject.value = ''
  body.value = ''
  priority.value = false
  selectedFile.value = null
  hasAttemptedSubmit.value = false

  if (editorRef.value) editorRef.value.innerHTML = ''
  if (fileInput.value) fileInput.value.value = ''
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

      <div v-if="selectedFile" class="attachments-section">
        <div class="attachment-chip">
          <span class="file-name">{{ selectedFile.name }}</span>
          <span class="file-size"> ({{ (selectedFile.size / 1024).toFixed(1) }} KB) </span>
        </div>
      </div>

      <div class="buttons">
        <div class="upload-btn">
          <input ref="fileInput" type="file" hidden @change="uploadFile" />
          <button type="button" @click="fileInput?.click()">Upload</button>
        </div>

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
  white-space: nowrap;
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
}
.toolbar button:hover {
  background-color: #e2dacb;
}
.text-editor {
  min-height: 250px;
  max-height: 300px;
  overflow-y: auto;
}
.text-editor:empty:before {
  content: attr(placeholder);
  color: #a09885;
}
.attachments-section {
  margin-top: 15px;
  display: flex;
  gap: 10px;
}
.attachment-chip {
  display: flex;
  align-items: center;
  gap: 10px;
  background-color: #fffdf9;
  border: 1px solid #c8c0ae;
  border-radius: 6px;
  padding: 8px 14px;
}
.file-size {
  font-size: 0.85rem;
  color: #666;
}
.send-btn,
.upload-btn button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: darkolivegreen;
  color: white;
  font-weight: 700;
  cursor: pointer;
}
.send-btn:hover,
.upload-btn button:hover {
  background-color: #506f3a;
}
.send-btn.btn-disabled {
  background-color: #8c9c84;
  cursor: not-allowed;
}
.buttons {
  display: flex;
  align-items: center;
  margin-top: 20px;
  gap: 20px;
}
.buttons label {
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
}
</style>
