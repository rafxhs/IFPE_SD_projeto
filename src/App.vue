<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const userInput = ref('')
const debouncedUserInput = ref('')
const translatedText = ref('')
const baseLanguage = ref('pt')
const targetLanguage = ref('en')
let timer = null

function clearInput() {
  userInput.value = '' 
  translatedText.value = ''
}

async function translate() {
  const response = await axios.post('http://localhost:3000/translate', {
    text: debouncedUserInput.value,
    sourceLanguage: baseLanguage.value,
    targetLanguage: targetLanguage.value
  })
  translatedText.value = response.data.translatedText
}

watch(userInput, (val) => {
  clearTimeout(timer)
  timer = setTimeout(() => {
    debouncedUserInput.value = val
    translate()
  }, 500)
})

watch([baseLanguage, targetLanguage], () => {
  if (debouncedUserInput.value) {
    translate()
  }
})

</script>

<template>
<body>
  <div class="flex flex-col h-screen w-screen items-start gap-4 bg-white p-10">
    <h1>Tradutor SD</h1>
    <div class="flex flex-row space-x-4">   
      <div class="relative rounded-lg border border-gray-200">

         <select v-model="baseLanguage" class="absolute top-2 left-2 rounded-full text-gray-800 py-2 px-4 text-sm bg-white border border-gray-200" id="baseLanguageSelect">
          <option value="pt" selected>Português</option>
          <option value="en">Inglês</option>
          <option value="es">Espanhol</option>
          <option value="fr">Francês</option>
        </select>

        <br>

        <textarea
        v-model="userInput"
        class="w-[623px] h-[164px] mt-2 p-4 pr-12 p-4 bg-white text-gray-800 resize-none focus:outline-none focus:ring-0"
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

  <div class="w-[623px] h-[auto] p-4 pr-12 rounded-lg p-4 bg-gray-200 shadow break-words whitespace-normal">
    <select v-model="targetLanguage" class="top-2 left-2 rounded-full text-gray-800 py-2 px-4 text-sm bg-white border border-gray-200" id="targetLanguageSelect">
          <option value="pt" selected>Português</option>
          <option value="en">Inglês</option>
          <option value="es">Espanhol</option>
          <option value="fr">Francês</option>
        </select>
        <br>
        <p class="mt-2">
          {{ translatedText || 'Tradução' }}
        </p>
  </div>

    </div>
  </div>
</body>
</template>

