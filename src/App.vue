<script setup>
import { ref } from 'vue'
const userInput = ref('')

function clearInput() {
  userInput.value = '' 
}

</script>

<template>
<body>
  <div class="flex flex-col h-screen w-screen items-start gap-4 bg-white p-10">
    <h1>Tradutor SD</h1>
    <div class="flex flex-row space-x-4">   
      <div class="relative">

        <textarea
        v-model="userInput"
        class="w-[623px] h-[164px] p-4 pr-12 rounded-lg border border-gray-200 p-4 bg-white text-gray-800 resize-none focus:outline-none focus:ring-0"
        />
      
        <button
          @click="clearInput"
          class="absolute top-2 right-2 rounded-full text-gray-800 py-2 px-2 text-sm hover:bg-gray-200"
          aria-label="Fechar"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" 
            stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
          </svg>
        </button>
  </div>

  <div class="w-[623px] h-[164px] p-4 pr-12 rounded-lg p-4 bg-gray-200 shadow break-words whitespace-normal">
    {{ userInput || 'Tradução' }}
  </div>

    </div>
  </div>
</body>
</template>

