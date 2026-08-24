<script setup>
import { ref, computed } from 'vue'

import MessageSearch from './components/MessageSearch.vue'
import MessageFilter from './components/MessageFilter.vue'
import MessageReader from './components/MessageReader.vue'
import MessageCard from './components/MessageCard.vue'

import { TestMessages } from '@/MessageData/MessageData'

const messages = ref(TestMessages)
const currentMessage = ref(null)

const filter = ref({
  status: 'All Messages',
  sort: 'Oldest',
})

const filteredMessages = computed(() => {
  let result = messages.value

  if (filter.value.status !== 'All Messages') {
    result = result.filter((message) => message.status === filter.value.status)
  }

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
      <div class="sidebar-controls">
        <MessageSearch />
        <MessageFilter @FilterApplied="filter = $event" />
      </div>

      <div class="MessageList">
        <MessageCard
          v-for="message in filteredMessages"
          :key="message.id"
          :message="message"
          @openMail="currentMessage = $event"
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
  width: 100%;
  max-width: 350px;
  height: 85vh;
  flex-shrink: 0;
  margin-top: 15vh;
  gap: 5px;
}
.sidebar-controls {
  display: flex;
  flex-direction: column;
  width: 100%;
  flex-shrink: 0;
  gap: 5px;
}
.MessageList {
  display: flex;
  flex-direction: column;
  gap: 5px;
  width: 100%;
  overflow-y: auto;
  flex-grow: 1;
}
</style>
