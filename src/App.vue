<template>
  <div class="wrapper">
    <div class="wrapper-content">
      
      <section>
        <div class="containter">
          <h1>{{ title }}</h1>


          <!-- показывается только тогда, когда значение data message - true -->
          <!-- если true, то вызываем значение massage -->
          <message v-if="message" :message="message" />
          
          <!-- new note -->
          <!-- в пропс :note передаём всю note из data -->
          <!-- @addNote - название эмита и то, что мы будем делать, когда этот эмит будет передаваться -->
          <!-- передавать будем addNote -->
          <newNote
            :note="note"
            @addNote="addNote" />
            

          <!-- note list -->
          <div class="notes">
            <div class="note" v-for="(note, index) in notes" :key="index">
              <div class="note-header">
                <p>{{ note.title}}</p>
              </div>
              <div class="note-body">
                <p>{{ note.descr }}</p>
                <span>{{ note.date }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

    </div>
  </div>
</template>

<script>
import message from '@/components/Message.vue'
// компоненты принято называть с большой буквы; при импорте - с маленькой
import newNote from '@/components/NewNote.vue'
export default {
  components: {
    message, newNote
  },
  data() {
    return {
      title: 'Notes App',
      message: null,
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
      }
    }
}
</script>

<style>
</style>
