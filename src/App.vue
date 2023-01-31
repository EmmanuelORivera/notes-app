<script setup>
import { ref } from 'vue'
const showModal = ref(false)
const newNote = ref('')
const errorMessage = ref('')
const notes = ref([])

const generateRandomLightColor = () => {
  return `hsl(${Math.random() * 360}, 100%, 75%)`
}

const addNote = () => {
  notes.value.push({
    id: new Date().getTime(),
    text: newNote.value,
    date: new Date(),
    backgroundColor: generateRandomLightColor(),
  })
}

const cleanNoteState = () => {
  showModal.value = false
  newNote.value = ''
}

const handleInput = () => {
  errorMessage.value =
    newNote.value.length > 0 && newNote.value.length < 10
      ? 'Notes must to be at least 10 characters'
      : ''
}

const saveNote = () => {
  addNote()
  cleanNoteState()
}
const isValidNote = () => {
  return newNote.value.length > 9
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          @input="handleInput"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button
          :class="{ isDisabled: !isValidNote() }"
          @click="saveNote()"
          :disabled="!isValidNote()"
        >
          Add Note
        </button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ background: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString() }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2.5em;
}

h1 {
  margin: 0;
  font-weight: bold;
  font-size: 4em;
}

header button {
  border: none;
  border-radius: 100%;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  color: white;
  font-size: 20px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 8px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}
.overlay {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.77);
  z-index: 10;
}

.modal {
  display: flex;
  flex-direction: column;
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
}
.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

button.isDisabled {
  cursor: auto;
  background-color: rgb(188, 144, 229);
}
.modal .close {
  background: rgb(195, 0, 0);
}
.modal p {
  color: rgb(195, 0, 0);
}
</style>
