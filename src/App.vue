<script setup lang="ts">
interface Obj {
  id: number
  text: string
  date: Date
  color: string
}

import { ref } from 'vue'
const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref<Obj[]>([])

const addNote = () => {
  if (newNote.value.length < 8) {
    return errorMessage.value = "Note needs to be atleast 8 characters."
  } else if (newNote.value.length > 150) {
    return errorMessage.value = "Exceeded 150 characters limit."
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    color: "hsl(" + Math.random() * 360 + ", 100%, 80%)",
  })
  showModal.value = false
  newNote.value = ""
  errorMessage.value = ""
}
</script>

<template>
  <div v-if="showModal" class="absolute w-full h-full z-10 bg-black/80 flex justify-center items-center">
    <div class="relative w-1/2 bg-white rounded-lg flex flex-col p-9 gap-2">
      <div class="flex justify-center items-center  absolute top-1 right-1  rounded-full w-8 h-8 hover:bg-gray-100">
        <button @click="showModal = false"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <path fill="currentColor" d="m12 13.4l-4.9 4.9q-.275.275-.7.275t-.7-.275q-.275-.275-.275-.7t.275-.7l4.9-4.9l-4.9-4.9q-.275-.275-.275-.7t.275-.7q.275-.275.7-.275t.7.275l4.9 4.9l4.9-4.9q.275-.275.7-.275t.7.275q.275.275.275.7t-.275.7L13.4 12l4.9 4.9q.275.275.275.7t-.275.7q-.275.275-.7.275t-.7-.275L12 13.4Z" />
          </svg>
        </button>
      </div>
      <textarea class="bg-gray-50 border border-gray-500 rounded p-2" name="note" id="note" cols="30" rows="7" v-model.trim="newNote"></textarea>
      <p class="text-rose-600" v-if="errorMessage">{{ errorMessage }}</p>
      <button class="bg-purple-500 text-white py-2 rounded" @click="addNote">Add Note</button>
    </div>
  </div>
  <div class="container mx-auto">
    <header class="py-8 flex justify-between items-center">
      <h1 class="font-bold text-6xl text-gray-900">Notes</h1>
      <button class="w-12 h-12 rounded-full text-white font-bold bg-gray-900 text-3xl" @click="showModal = !showModal">+</button>
    </header>
    <div class="flex flex-wrap">
      <div v-for="note in notes" :key="note.id" class="overflow-hidden h-56 w-56 rounded-2xl flex flex-col justify-between mr-5 mb-5 p-2" :style="{ backgroundColor: note.color }">
        <p>{{ note.text }}</p>
        <p class="text-xs font-bold">{{ note.date.toLocaleDateString("en-UK") }}</p>
      </div>
    </div>
  </div>
</template>
