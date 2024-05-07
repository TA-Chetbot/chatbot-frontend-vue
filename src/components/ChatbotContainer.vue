<script setup>
import { ref } from 'vue'
import ChatbotChatbox from './ChatbotChatbox.vue'
import ChatbotToggle from './ChatbotToggle.vue'

const isChatboxOpen = ref(false)

const toggleChatbox = () => {
  isChatboxOpen.value = !isChatboxOpen.value
}
</script>

<template>
  <div class="fixed bottom-5 right-5 z-50 flex flex-col items-end gap-3">
    <ChatbotChatbox
      :isChatboxOpen="isChatboxOpen"
      :closeChatbox="toggleChatbox"
    />
    <ChatbotToggle
      :isChatboxOpen="isChatboxOpen"
      :toggleChatbox="toggleChatbox"
    />
  </div>
</template>