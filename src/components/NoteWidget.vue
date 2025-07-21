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
        <button class="add-button" @click="addNote()">Add</button>

        <ul>
            <li v-for="note in notes" :key="note.id">
                {{ note.text }}
                <button class="add-button" @click="removeNote(note.id)">X</button>
            </li>
        </ul>
    </div>
</template>



<style scoped>
.note-widget {
  background: var(--widget-bg);
  border: 2px solid var(--widget-border);
  border-radius: 6px;
  padding: 1em;
  margin-bottom: 1em;
  color: var(--fg);
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
  border-bottom: 1px solid rgba(0,0,0,0.1);
}

.new-note {
  font-size: 1.1rem;
  padding: 0.5em;
  border: 1px solid var(--widget-border);
  border-radius: 4px;
  margin-right: 0.5em;
  background-color: var(--bg);
  color: var(--fg);
  transition: background-color 0.2s, color 0.2s;
}

.add-button {
  background-color: var(--button-bg);
  color: var(--fg);
  border: 1px solid var(--button-border-hover);
  border-radius: 4px;
  padding: 0.5em 1em;
  cursor: pointer;
  transition: border-color 0.2s, background-color 0.2s;
}
.add-button:hover {
  border-color: var(--widget-border);
}

.remove-button {
  background: transparent;
  border: none;
  color: var(--fg);
  font-weight: bold;
  cursor: pointer;
  margin-left: 0.5em;
}
</style>
