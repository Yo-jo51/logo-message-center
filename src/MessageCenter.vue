<script setup>
import { ref } from 'vue'
import MessageSearch from './components/MessageSearch.vue'
import MessageFilter from './components/MessageFilter.vue'
import MessageList from './components/MessageList.vue'
import MessageReader from './components/MessageReader.vue'
import MessageCard from './components/MessageCard.vue'

import { TestMessages } from '@/MessageData/MessageData'

const currentMessage = ref(null)

const showReader = ref(false)
</script>

<template>
  <div class="layout">
    <div class="sidebar">
      <MessageSearch />
      <MessageFilter />

      <div class="MessageList">
        <MessageCard
          v-for="message in TestMessages"
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
  padding-top: 15vh;
  display: flex;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.sidebar {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  max-width: 350px;
  height: 100vh;
  flex-shrink: 0;
}

.MessageList {
  display: flex;
  flex-direction: column;
  gap: 5px;
  width: 100%;
  margin-top: 80px;
  overflow-y: auto;
}

.MessageList::-webkit-scrollbar {
  display: none;
}
</style>
