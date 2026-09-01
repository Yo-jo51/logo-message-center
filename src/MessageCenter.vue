<script setup>
import { ref, computed } from 'vue'

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

const filterStatus = ref('All Messages')
const sortOrder = ref('Oldest')

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

function applyFilter(filter) {
  filterStatus.value = filter.status
  sortOrder.value = filter.sort
}

const filteredMessages = computed(() => {
  let result = [...messages.value]

  if (filterStatus.value === 'Seen') {
    result = result.filter((message) => message.seen)
  }

  if (filterStatus.value === 'Unseen') {
    result = result.filter((message) => !message.seen)
  }

  if (filterStatus.value === 'Priority') {
    result = result.filter((message) => message.important)
  }

  const search = searchText.value.trim().toLowerCase()

  if (search) {
    result = result.filter(
      (message) =>
        String(message.sender || '')
          .toLowerCase()
          .includes(search) ||
        String(message.subject || '')
          .toLowerCase()
          .includes(search) ||
        String(message.body || '')
          .toLowerCase()
          .includes(search),
    )
  }

  result.sort((a, b) => {
    const dateA = new Date(a.timestamp)
    const dateB = new Date(b.timestamp)

    if (sortOrder.value === 'Newest') {
      return dateB - dateA
    }

    return dateA - dateB
  })

  return result
})

const inboxMessages = computed(() =>
  filteredMessages.value.filter((message) => message.folder === 'Inbox'),
)

const sentMessages = computed(() =>
  filteredMessages.value.filter((message) => message.folder === 'Sent'),
)

const sendMail = (newMailData) => {
  messages.value.push({
    id: messages.value.length + 1,
    sender: 'You',
    reciever: newMailData.recipient,
    subject: newMailData.subject,
    body: newMailData.body,
    timestamp: new Date().toDateString(),
    folder: 'Sent',
    seen: true,
    important: newMailData.priority,
  })

  creatingNewMail.value = false
}

//High priority nachrichten zählen
const priorityCount = computed(() => {
  return messages.value.filter((message) => message.important).length
})
</script>

<template>
  <div class="layout">
    <div class="sidebar">
      <MessageSearch @SearchChanged="searchText = $event" />

      <MessageFilter
        :total-count="messages.length"
        :unread-count="messages.filter((m) => !m.seen).length"
        :read-count="messages.filter((m) => m.seen).length"
        :prioritycount="priorityCount"
        @FilterApplied="applyFilter"
      />

      <!-- Inbox -->
      <div class="folder" :class="{ active: inboxOpen }">
        <button class="folder-header" @click="toggleInbox">
          <span>Inbox ({{ inboxMessages.length }})</span>

          <span>
            {{ inboxOpen ? '∧' : '∨' }}
          </span>
        </button>

        <div v-show="inboxOpen" class="folder-body">
          <MessageCard
            v-for="message in inboxMessages"
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

      <!-- sent -->
      <div class="folder" :class="{ active: sentOpen }">
        <button class="folder-header" @click="toggleSent">
          <span>Sent ({{ sentMessages.length }})</span>

          <span>
            {{ sentOpen ? '∧' : '∨' }}
          </span>
        </button>

        <div v-show="sentOpen" class="folder-body">
          <MessageCard
            v-for="message in sentMessages"
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

    <MessageReader v-else-if="currentMessage" :message="currentMessage" />

    <div v-else class="empty-reader">No Message selected</div>

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

.folder {
  border: none;
  overflow: hidden;
}

.folder.active {
  border-bottom: 1px solid #87a687;
}

.folder-header {
  width: 100%;
  border: none;
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
  cursor: pointer;
}

.plus-btn:hover {
  background-color: #5a7b5a;
}

.empty-reader {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #777;
}
</style>
