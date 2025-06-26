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
    <nav class="fixed top-0 left-0 w-full bg-blue-50 shadow z-50">
      <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
        <h1 class="text-xl font-semibold text-blue-800">LangFlow</h1>
        <div class="space-x-4">
          <a href="#" class="text-gray-600 hover:text-blue-950 text-sm">Início</a>
          <a href="#" class="text-gray-600 hover:text-blue-950 text-sm">Histórico</a>
          <a href="#" class="text-gray-600 hover:text-blue-950 text-sm">Idiomas</a>
        </div>
      </div>
    </nav>
  <div class="flex flex-col min-h-screen w-screen items-center gap-4 bg-white pt-24 p-10 z-0">
    <div class="flex flex-row justify-center space-x-4">   
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
  </div>

  <div class="w-[623px] h-[auto] p-4 pr-12 rounded-lg p-4 bg-gray-100 shadow break-words whitespace-normal">
    <select v-model="targetLanguage" class="top-2 left-2 rounded-full text-gray-800 py-2 px-4 text-sm bg-white border border-gray-200" id="targetLanguageSelect">
          <option value="pt" selected>Português</option>
          <option value="en">Inglês</option>
          <option value="es">Espanhol</option>
          <option value="fr">Francês</option>
        </select>
        <br>
        <p class="my-4">
          {{ translatedText || 'Tradução' }}
        </p>
  </div>

    </div>
  </div>
</body>
</template>

