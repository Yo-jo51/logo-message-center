<script setup>
import { ref } from 'vue'

import MessageSearch from './components/MessageSearch.vue'
import MessageFilter from './components/MessageFilter.vue'
import MessageReader from './components/MessageReader.vue'
import MessageCard from './components/MessageCard.vue'
import NewMail from './components/NewMail.vue'

import { TestMessages } from '@/MessageData/MessageData'

const messages = ref(TestMessages)
const searchText = ref('')
const currentMessage = ref(null)
const creatingNewMail = ref(false)

const inboxOpen = ref(true)
const sentOpen = ref(true)

function openNewMail() {
  currentMessage.value = null
  creatingNewMail.value = true
}

function openMessage(message) {
  message.seen = true
  currentMessage.value = message
  creatingNewMail.value = false
}

function toggleInbox() {
  inboxOpen.value = !inboxOpen.value
}

function toggleSent() {
  sentOpen.value = !sentOpen.value
}

const sendMail = (newMailData) => {
  const newMessage = {
    id: messages.value.length + 1,
    sender: 'You',
    subject: newMailData.subject,
    body: newMailData.body,
    timestamp: new Date().toLocaleDateString(),
    folder: 'Sent',
    seen: true,
  }

  messages.value.push(newMessage)

  creatingNewMail.value = false
}
</script>

<template>
  <div class="layout">
    <div class="sidebar">
      <MessageSearch @SearchChanged="searchText = $event" />

      <MessageFilter />

      <div class="UnreadCounter">
        Unread Messages:
        {{ messages.filter((message) => !message.seen).length }}
      </div>

      <!-- INBOX -->
      <div class="folder">
        <button class="folder-header" @click="toggleInbox">
          <span>Inbox</span>

          <span>
            {{ messages.filter((message) => message.folder === 'Inbox').length }}
          </span>

          <span>
            {{ inboxOpen ? '∧' : '∨' }}
          </span>
        </button>

        <div v-show="inboxOpen" class="folder-body">
          <MessageCard
            v-for="message in messages.filter((message) => message.folder === 'Inbox')"
            :key="message.id"
            :message="message"
            @openMail="openMessage"
            :class="{
              seen: message.seen,
              important: message.important,
              selected: currentMessage === message,
            }"
          />
        </div>
      </div>

      <!-- SENT -->
      <div class="folder">
        <button class="folder-header" @click="toggleSent">
          <span>Sent</span>

          <span>
            {{ messages.filter((message) => message.folder === 'Sent').length }}
          </span>

          <span>
            {{ sentOpen ? '∧' : '∨' }}
          </span>
        </button>

        <div v-show="sentOpen" class="folder-body">
          <MessageCard
            v-for="message in messages.filter((message) => message.folder === 'Sent')"
            :key="message.id"
            :message="message"
            @openMail="openMessage"
            :class="{
              seen: message.seen,
              important: message.important,
              selected: currentMessage === message,
            }"
          />
        </div>
      </div>
    </div>

    <NewMail v-if="creatingNewMail" @send="sendMail" />

    <MessageReader v-else :message="currentMessage" />

    <button class="plus-btn" @click="openNewMail">+</button>
  </div>
</template>

<style scoped>
.layout {
  display: flex;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.sidebar {
  display: flex;
  flex-direction: column;
  width: 350px;
  height: 85vh;
  margin-top: 15vh;
  gap: 5px;
}

.MessageList {
  display: flex;
  flex-direction: column;
  gap: 8px;
  width: 100%;
  overflow-y: auto;
  scrollbar-width: none;
  flex-grow: 1;
  padding: 5px;
}

.folder {
  border-bottom: 1px solid #87a687;
  overflow: hidden;
}

.folder-header {
  width: 100%;
  border: 1px solid #87a687;
  border-radius: 6px;
  background: #edf3ed;
  color: #2f4f2f;
  padding: 10px 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.9rem;
  font-weight: 700;
  cursor: pointer;
  text-align: left;
}

.folder-header span:first-child {
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.folder-meta {
  color: #4f6e4f;
  margin-right: 8px;
  font-size: 0.8rem;
}

.folder-toggle {
  width: 18px;
  text-align: center;
  font-size: 1.2rem;
  line-height: 1;
}

.folder-body {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 6px;
  max-height: 400px;
  overflow-y: auto;
  scrollbar-width: none;
}

.UnreadCounter {
  text-align: center;
  padding: 6px 12px;
  font-weight: 600;
  font-size: 0.85rem;
  background-color: #fffdf9;
  border: 1px solid #87a687;
  color: darkolivegreen;
  border-radius: 3px;
  align-self: center;
}

.plus-btn {
  position: fixed;
  right: 30px;
  bottom: 30px;
  width: 55px;
  height: 55px;
  border: none;
  border-radius: 50%;
  background-color: #4f6e4f;
  color: white;
  font-size: 32px;
  font-weight: 300;
  line-height: 1;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.plus-btn:hover {
  background-color: #5a7b5a;
}
</style>
