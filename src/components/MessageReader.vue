<script setup lang="ts">
interface Attachment {
  name: string
  path: string
  size: string
}

defineProps<{
  message: {
    sender: string
    reciever: string | null
    timestamp: string
    subject: string
    body: string | null
    attachments?: Attachment[]
    fileName?: string
  } | null
}>()
</script>

<template>
  <div
    class="flex-1 h-[85vh] bg-[var(--color-background)] p-5 mt-[15vh] overflow-y-auto flex flex-col"
    :class="{ 'items-center justify-center text-center': !message }"
  >
    <div v-if="message" class="w-full">
      <p class="text-lg my-[2px]">From: {{ message.sender }}</p>
      <p class="text-lg my-[2px]">To: {{ message.reciever || 'you' }}</p>

      <p class="mt-[6px] mb-[6px] text-[var(--color-text-secondary)]">
        Date: {{ message.timestamp }}
      </p>

      <h2
        class="pt-[6px] border-t border-[var(--color-border)] text-2xl font-bold text-[var(--color-text)]"
      >
        {{ message.subject }}
      </h2>
      <p
        class="mt-5 max-w-[70ch] break-words text-[var(--color-text)]"
        v-html="message.body || 'No Content'"
      ></p>

      <div
        v-if="(message.attachments && message.attachments.length > 0) || message.fileName"
        class="mt-10 pt-[15px] border-t border-dashed border-[var(--color-border)]"
      >
        <div class="flex flex-col gap-2 items-start">
          <a
            v-for="file in message.attachments || []"
            :key="file.path"
            :href="file.path"
            :download="file.name"
            class="flex items-center gap-2.5 bg-[var(--color-surface)] border-2 border-[var(--color-border)] rounded-xl px-3.5 py-2 min-w-[180px] max-w-[280px] shadow-[0_2px_4px_rgba(23,32,51,0.03)] no-underline hover:bg-[var(--color-surface-hover)] hover:border-[var(--color-primary)] transition-colors"
          >
            <span
              class="text-[0.85rem] font-semibold text-[var(--color-text)] whitespace-nowrap overflow-hidden text-ellipsis"
            >
              📥 {{ file.name }}
            </span>
            <span
              class="whitespace-nowrap text-[0.75rem] text-[var(--color-text-secondary)] mt-[2px]"
            >
              {{ file.size }}
            </span>
          </a>

          <div
            v-if="message.fileName && (!message.attachments || message.attachments.length === 0)"
            class="flex items-center gap-[10px] bg-[var(--color-surface)] border border-[var(--color-border)] rounded-md px-[14px] py-2 min-w-[180px] max-w-[280px] shadow-[0_2px_4px_rgba(23,32,51,0.03)]"
          >
            <span
              class="text-[0.85rem] font-semibold text-[var(--color-text)] whitespace-nowrap overflow-hidden text-ellipsis"
            >
              📥 {{ message.fileName }}
            </span>
            <span
              class="whitespace-nowrap text-[0.75rem] text-[var(--color-text-secondary)] mt-[2px]"
            >
              Uploaded
            </span>
          </div>
        </div>
      </div>
    </div>

    <h3
      v-else
      class="font-black text-xl max-w-[45ch] text-[var(--color-text-muted)] -translate-y-5"
    >
      No email selected yet. Please click on an email from the list to view it in the reader
    </h3>
  </div>
</template>
