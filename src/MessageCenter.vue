<script setup>
import { ref, computed } from 'vue'

import MessageSearch from './components/MessageSearch.vue'
import MessageFilter from './components/MessageFilter.vue'
import MessageReader from './components/MessageReader.vue'
import MessageCard from './components/MessageCard.vue'
import NewMail from './components/NewMail.vue'

import { TestMessages } from '@/MessageData/MessageData'

const saved = localStorage.getItem('messages')
const messages = ref(saved ? JSON.parse(saved) : TestMessages)

const searchText = ref('')
const currentMessage = ref(null)
const creatingNewMail = ref(false)

const inboxOpen = ref(true)
const sentOpen = ref(true)

const filterStatus = ref('All Messages')
const sortOrder = ref('Newest')

const filteredMessages = computed(() => {
  let result = [...messages.value]

  if (filterStatus.value === 'Seen') result = result.filter((m) => m.seen)
  if (filterStatus.value === 'Unseen') result = result.filter((m) => !m.seen)
  if (filterStatus.value === 'Priority') result = result.filter((m) => m.important)

  const search = searchText.value.toLowerCase().trim()

  if (search) {
    result = result.filter((m) =>
      `${m.sender} ${m.subject} ${m.body}`.toLowerCase().includes(search),
    )
  }

  result.sort((a, b) => {
    const dateA = new Date(a.timestamp)
    const dateB = new Date(b.timestamp)

    return sortOrder.value === 'Newest' ? dateB - dateA : dateA - dateB
  })

  return result
})

const inboxMessages = computed(() => filteredMessages.value.filter((m) => m.folder === 'Inbox'))
const sentMessages = computed(() => filteredMessages.value.filter((m) => m.folder === 'Sent'))
const priorityCount = computed(() => messages.value.filter((m) => m.important).length)

function openMessage(message) {
  message.seen = true
  currentMessage.value = message
  creatingNewMail.value = false

  saveMessages()
}

function openNewMail() {
  currentMessage.value = null
  creatingNewMail.value = true
}

function applyFilter(filter) {
  filterStatus.value = filter.status
  sortOrder.value = filter.sort
}

function saveMessages() {
  localStorage.setItem('messages', JSON.stringify(messages.value))
}

function sendMail(mail) {
  const id = messages.value.length ? Math.max(...messages.value.map((m) => m.id)) + 1 : 1

  let extrahiertName = ''
  if (mail.fileName) {
    extrahiertName = mail.fileName
  } else if (mail.attachments && mail.attachments.length > 0) {
    const firstAttachment = mail.attachments[0]
    extrahiertName = firstAttachment.name || firstAttachment
  }

  console.log(mail.attachments)

  messages.value.push({
    id,
    sender: 'You',
    receiver: mail.recipient,
    subject: mail.subject,
    body: mail.body,
    timestamp: new Date().toDateString(),
    folder: 'Sent',
    seen: true,
    important: mail.priority,
  })

  saveMessages()
  creatingNewMail.value = false
}

function toggleInbox() {
  inboxOpen.value = !inboxOpen.value
}

function toggleSent() {
  sentOpen.value = !sentOpen.value
}
</script>

<template>
  <div class="flex w-screen h-screen overflow-hidden">
    <div class="flex flex-col w-[350px] h-[85vh] mt-[15vh] gap-[5px]">
      <MessageSearch @SearchChanged="searchText = $event" />

      <MessageFilter
        :total-count="messages.length"
        :unread-count="messages.filter((m) => !m.seen).length"
        :read-count="messages.filter((m) => m.seen).length"
        :prioritycount="priorityCount"
        @FilterApplied="applyFilter"
      />

      <div
        class="overflow-hidden"
        :class="{
          'border-b border-[var(--color-border-dark)]': inboxOpen,
        }"
      >
        <button
          class="w-full border-none text-[var(--color-text)] py-[10px] px-3 flex justify-between items-center text-[0.9rem] font-bold cursor-pointer text-left"
          @click="toggleInbox"
        >
          <span>Inbox ({{ inboxMessages.length }})</span>

          <svg
            v-if="inboxOpen"
            xmlns="http://w3.org"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="m18 15-6-6-6 6" />
          </svg>
          <svg
            v-else
            xmlns="http://w3.org"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="m6 9 6 6 6-6" />
          </svg>
        </button>

        <div
          v-show="inboxOpen"
          class="flex flex-col gap-[6px] p-[6px] max-h-[270px] overflow-y-auto [scrollbar-width:none]"
        >
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

      <div
        class="overflow-hidden"
        :class="{
          'border-b border-[var(--color-border-dark)]': sentOpen,
        }"
      >
        <button
          class="w-full border-none text-[var(--color-text)] py-[10px] px-3 flex justify-between items-center text-[0.9rem] font-bold cursor-pointer text-left"
          @click="toggleSent"
        >
          <span>Sent ({{ sentMessages.length }})</span>

          <svg
            v-if="sentOpen"
            xmlns="http://w3.org"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="m18 15-6-6-6 6" />
          </svg>
          <svg
            v-else
            xmlns="http://w3.org"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="m6 9 6 6 6-6" />
          </svg>
        </button>

        <div
          v-show="sentOpen"
          class="flex flex-col gap-[6px] p-[6px] max-h-[270px] overflow-y-auto [scrollbar-width:none]"
        >
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

    <div v-else class="flex-1 flex items-center justify-center text-[var(--color-text-muted)]">
      No Message selected
    </div>

    <button
      class="fixed right-[30px] bottom-[30px] size-[55px] border-none rounded-full bg-[var(--color-primary)] cursor-pointer flex justify-center items-center box-border text-white"
      @click="openNewMail"
    >
      <span class="text-4xl font-extrabold h-11">+</span>
    </button>
  </div>
</template>
