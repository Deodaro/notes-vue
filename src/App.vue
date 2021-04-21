<template>
  <div class="wrapper">
    <div class="wrapper-content">
      
      <section>
        <div class="containter">

          <!-- показывается только тогда, когда значение data message - true -->
          <!-- если true, то вызываем значение massage -->
          <message v-if="message" :message="message" />
          
          <!-- new note -->
          <!-- в пропс :note передаём всю note из data -->
          <!-- @addNote - название эмита и то, что мы будем делать, когда этот эмит будет передаваться -->
          <!-- передавать будем addNote -->
          <newNote :note="note" @addNote="addNote" />

          <!-- title -->
          <div class="note-header">
            <h1>{{ title }}</h1>
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" style="cursor: pointer" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" style="cursor: pointer" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>
          
          <!-- note list -->
          <notes :notes="notes" :grid="grid" @remove="removeNote" />

        </div>
      </section>

    </div>
  </div>
</template>

<script>
import message from '@/components/Message.vue'
// компоненты принято называть с большой буквы; при импорте - с маленькой
import newNote from '@/components/NewNote.vue'
import notes from '@/components/Notes.vue'

export default {
  components: {
    message, notes, newNote
  },
  data() {
    return {
      title: 'Notes App',
      message: null,
      grid: true,
      // для новой заметки
      note: {
        title: '',
        descr: ''
      },
      // в будущем эти заметки будут храниться на сервере и подгружаться оттуда
      notes: [
        {
          title: 'First Note',
          descr: 'Description for first note',
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Second Note',
          descr: 'Description for second note',
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Third Note',
          descr: 'Description for third note',
          date: new Date(Date.now()).toLocaleString()
        }
      ]
    }
  },
  methods: {
      addNote () {
        // создаём переменные из this.note, чтобы не набирать каждый раз 'this.note.что-то'
        let {title, descr} = this.note

        if (title === '') {
          this.message = 'title can\'t be blank!'
          return false
        }

        this.notes.push({
          title,
          descr,
          date: new Date(Date.now()).toLocaleString()
        })
        this.message = null
        this.note.title = ''
        this.note.descr = ''
        // но ведь объяление переменных {title, descr} и reset происходят у нас на одном и том же уровне вложенности?
      },
      removeNote (index) {
        this.notes.splice(index, 1)
      }
    }
}
</script>

<style>
</style>
