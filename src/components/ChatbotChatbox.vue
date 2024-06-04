<script setup>
import { ref } from 'vue'

const props = defineProps({
  isChatboxOpen: Boolean,
  closeChatbox: Function
})

const prompt = ref('')
const isTyping = ref(false)
const chatLog = ref([])

async function getAnswer(question) {
  isTyping.value = true
  chatLog.value.push({ role: 'user', content: question })
  const preprocessed_question = await fetch(`${import.meta.env.VITE_API_URL}/preprocess_question`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ text: question })
  })

  const preprocessed_question_json = await preprocessed_question.json();
  console.log(preprocessed_question_json)
  const response = await fetch(
    `${import.meta.env.VITE_API_URL}/get_answer`,
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        question: preprocessed_question_json.preprocessed_question,
      }),
    }
  );
  const data = await response.json()
  chatLog.value.push({ role: 'assistant', content: data.answer })
  isTyping.value = false
  return data
}

function sendPrompt(event) {
  if (event?.key === 'Enter' || event?.type === 'click') {
    getAnswer(prompt.value)
    prompt.value = ''
  }
}

function clearChatLog() {
  chatLog.value = []
}
</script>

<template>
  <div
    class="w-[340px] min-h-[600px] bg-white shadow-lg rounded-[25px] overflow-auto border origin-bottom-right transition duration-200"
    :class="{ 'scale-100': isChatboxOpen, 'scale-0': !isChatboxOpen }">
    <div
      class="w-full h-[70px] flex justify-between items-center px-[20px] border-b border-[#E2E2E2] shadow-[0px_42px_67px_0px_rgba(122,122,122,0.1)]">
      <p class="font-bold text-blue-500 text-xl">Chatbot</p>
      <img @click="closeChatbox" class="cursor-pointer" width="30" height="30" alt="exit"
        src="./icons/sign-out-alt.svg" />
    </div>
    <div class="h-[350px] pt-[16px] overflow-y-auto px-2">
      <div v-for="(chat, index) in chatLog" :key="index"
        :class="`chat ${chat.role === 'user' ? 'chat-end' : 'chat-start'}`">
        <div v-if="chat.role === 'assistant'" class="chat-image avatar">
          <div class="w-10 rounded-full">
            <img width="50" height="50" alt="Tailwind CSS chat bubble component" src="./icons/chatbot.png" />
          </div>
        </div>
        <div :class="`chat-bubble ${chat.role === 'user'
          ? 'bg-blue-200 text-slate-600'
          : 'bg-blue-500 text-slate-200'
          }`">
          {{ chat.content }}
        </div>
      </div>
      <div v-if="isTyping" class="chat chat-start">
        <div class="chat-image avatar">
          <div class="w-10 rounded-full">
            <img width="50" height="50" alt="Tailwind CSS chat bubble component" src="./icons/chatbot.png" />
          </div>
        </div>
        <div class="chat-bubble bg-blue-500 text-slate-200">Typing...</div>
      </div>
    </div>
    <div
      class="h-[180px] px-2 flex flex-col flex-1 justify-center items-center w-full border-t border-[#E2E2E2] shadow-[0px_-42px_67px_0px_rgba(122,122,122,0.1)]">
      <div class="w-[96%] h-[80%] p-[16px] bg-slate-100 rounded-[10px] flex flex-col justify-center overflow-hidden">
        <textarea @keydown.enter="sendPrompt" v-model="prompt"
          class="bg-slate-100 w-full h-[110px] resize-none text-xs text-slate-600" style="border: none; outline: none"
          placeholder="Ask me questions..."></textarea>
        <div class="flex justify-between items-center">
          <img @click="clearChatLog" class="cursor-pointer" width="30" height="30" alt="new-chat"
            src="./icons/comment-plus.svg" />
          <div @click="sendPrompt"
            class="w-[27px] h-[27px] rounded-full bg-blue-200 flex justify-center items-center cursor-pointer">
            <img width="500" height="500" alt="submit" src="./icons/angle-right-b.svg" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>