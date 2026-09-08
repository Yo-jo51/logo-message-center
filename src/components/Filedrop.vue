<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  (e: 'fileSelected', file: File): void
}>()

const isDragging = ref(false)
const fileInput = ref<HTMLInputElement | null>(null)

function openFilePicker() {
  fileInput.value?.click()
}

function handleFile(file: File | undefined) {
  if (!file) return

  emit('fileSelected', file)
}

function handleFileInput(event: Event) {
  const input = event.target as HTMLInputElement
  handleFile(input.files?.[0])
}

function handleDragOver() {
  isDragging.value = true
}

function handleDragLeave() {
  isDragging.value = false
}

function handleDrop(event: DragEvent) {
  isDragging.value = false
  handleFile(event.dataTransfer?.files[0])
}
</script>

<template>
  <div
    class="px-5 py-[10px] border-dashed border-2 border-[#00bfff] bg-[#0B2240] rounded-sm font-bold cursor-pointer text-white"
    :class="{ 'opacity-70': isDragging }"
    @click="openFilePicker"
    @dragover.prevent="handleDragOver"
    @dragleave="handleDragLeave"
    @drop.prevent="handleDrop"
  >
    <input ref="fileInput" type="file" class="hidden" @change="handleFileInput" />

    {{ isDragging ? 'Drop File' : 'Choose or Drop File' }}
  </div>
</template>
