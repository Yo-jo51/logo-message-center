<script setup>
import { ref, computed } from 'vue'

import MessageSearch from './components/MessageSearch.vue'
import MessageFilter from './components/MessageFilter.vue'
import MessageReader from './components/MessageReader.vue'
import MessageCard from './components/MessageCard.vue'

import { TestMessages } from '@/MessageData/MessageData'

const messages = ref(TestMessages)
const searchText = ref('')
const currentMessage = ref(null)

const filter = ref({
  status: 'All Messages',
  sort: 'Oldest',
})

function openMessage(message) {
  message.seen = true
  currentMessage.value = message
}

const filteredMessages = computed(() => {
  let result = messages.value

  if (searchText.value) {
    const search = searchText.value.toLowerCase()

    result = result.filter(
      (message) =>
        message.sender.toLowerCase().includes(search) ||
        message.subject.toLowerCase().includes(search) ||
        message.body.toLowerCase().includes(search),
    )
  }

  if (filter.value.status === 'Seen') result = result.filter((message) => message.seen)

  if (filter.value.status === 'Unseen') result = result.filter((message) => !message.seen)

  return [...result].sort((a, b) => {
    const timeA = new Date(a.timestamp)
    const timeB = new Date(b.timestamp)

    return filter.value.sort === 'Newest' ? timeB - timeA : timeA - timeB
  })
})
</script>

<template>
  <div class="layout">
    <div class="sidebar">
      <MessageSearch @SearchChanged="searchText = $event" />

      <MessageFilter @FilterApplied="filter = $event" />

      <div class="MessageList">
        <MessageCard
          v-for="message in filteredMessages"
          :key="message.id"
          :message="message"
          @openMail="openMessage"
          :class="{ ifseen: message.seen }"
        />
      </div>
    </div>

    <MessageReader :message="currentMessage" />
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
  gap: 5px;
  width: 100%;
  overflow-y: auto;
  flex-grow: 1;
  padding: 5px;
}
</style>
