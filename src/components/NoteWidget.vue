<script setup>
    import {ref, reactive, watchEffect} from 'vue';

    //Hold the current text in the input field
    const newNote = ref('')
    //Array of note objects: {id, text}
    const notes = reactive([])

    //Persist notes to localStorage whenever they change
    watchEffect(() => {
        localStorage.setItem('notes', JSON.stringify(notes))
    })

    //On load, seed notes from storage(if any)
    const saved = localStorage.getItem('notes')
    if (saved) {
        JSON.parse(saved).forEach(n => notes.push(n))
    }

    function addNote() {
        if(!newNote.value.trim()) return
        notes.push({id: Date.now(), text: newNote.value})
        newNote.value = ''
    }

    function removeNote(id)
    {
        const idx = notes.findIndex(n=>n.id === id)
        if(idx !== -1) notes.splice(idx, 1)
    }
</script>

<template>
    <div class="note-widget">
        <input
            type="text"
            :value="newNote"
            @input="e => newNote = e.target.value"
            placeholder="Type a note"
            class="new-note"
        />
        <button @click="addNote()">Add</button>

        <ul>
            <li v-for="note in notes" :key="note.id">
                {{ note.text }}
                <button @click="removeNote(note.id)">X</button>
            </li>
        </ul>
    </div>
</template>

<style scoped>

.note-widget{
    background: #eef6fd;
    border: 2px solid #4a90e2;
    border-radius: 6px;
    padding: 1em;
    margin-bottom: 1em;
    color: #2a3d66;
    font-weight: bold;
}

.note-widget ul {
  list-style: none;
  padding: 0;
  margin-top: 1rem;
}

.note-widget li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #ddd;
}

.new-note{
    font-size: 1.1rem
}
    /* .note-widget{
        border: 1px solid #ccc;
        padding: 1rem;
        margin-bottom: 1em
    }

    .note-widget ul{list-style: none; padding: 0;}
    .note-widget li{display: flex; justify-content: space-between;} */
</style>