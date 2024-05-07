<script setup lang="ts">
import './assets/background.css';
import ChatbotContainer from './components/ChatbotContainer.vue';
</script>

<template>
  <div class="w-screen h-screen background-pattern">
    <div className="w-full h-full flex flex-col justify-center items-center gap-[20px] text-center">
      <h1 className="text-black font-bold text-7xl">Tugas Akhir Chatbot Swarm Made with Vue</h1>
      <p className="text-xl text-slate-800">Click on the bottom right icon to toggle the Chatbot Interface</p>
    </div>
    <Suspense>
      <template #default>
        <ChatbotContainer />
      </template>
      <template #fallback>
        <div>Loading...</div>
      </template>
    </Suspense>
  </div>
</template>