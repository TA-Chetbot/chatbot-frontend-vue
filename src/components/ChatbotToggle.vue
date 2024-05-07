<script setup>
  const props = defineProps({
    isChatboxOpen: Boolean,
    toggleChatbox: Function
  })
</script>

<template>
  <div>
    <button
      class="bg-blue-500 text-white rounded-full w-16 h-16 flex items-center justify-center transition-transform duration-300"
      :class="{ 'transform rotate-45': isChatboxOpen }"
      @click="toggleChatbox"
      aria-label="Toggle Chatbot"
    >
      <span class="text-xl">&#128172;</span>
    </button>
  </div>
</template>
  