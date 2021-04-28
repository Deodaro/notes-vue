<template>
  <div class="wrapper">
    <div class="wrapper-content">
      
      <section>
        <div class="container">
          <!-- title -->
          <h1>{{ title }}</h1> 

          <!-- показывается только тогда, когда значение data message - true -->
          <!-- если true, то вызываем значение massage -->
          <message v-if="message" :message="message" />
          
          <!-- new note -->
          <!-- в пропс :note передаём всю note из data -->
          <!-- @addNote - название эмита и то, что мы будем делать, когда этот эмит будет передаваться -->
          <!-- передавать будем addNote -->
          <newNote :note="note" @addNote="addNote" />

          <div class="note-header">
            
            <!-- search -->
            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event" />

            <!-- <p>{{ search }}</p> -->
            
            <!-- icons controls -->
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" style="cursor: pointer" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" style="cursor: pointer" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>
          
          <!-- note list -->
          <notes :notes="notesFilter" :grid="grid" @remove="removeNote" />

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
import search from '@/components/Search.vue'

export default {
  components: {
    message, notes, newNote, search
  },
  data() {
    return {
      title: 'Notes App',
      search: '',
      message: null,
      grid: true,
      // для новой заметки
      note: {
        title: '',
        descr: ''
      },
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
    },
    computed: {
      notesFilter() {
        // массив, который будем обходить,
        // и строка, по которой будем фильтровать
        let array = this.notes,
          search = this.search
        // если search пустой, возвращаем array
        if (!search) return array
        search = search.trim().toLowerCase()
        // отфильтровать массив
        array = array.filter(function(item) {
          // если у заголовок item'а есть в search
          if (item.title.toLowerCase().indexOf(search) !== -1) {
            return item
          }
        })
        // проверка на ошибку
        return array
      }
    }
}
</script>

<style>

/* дублирует note-header */
/* .notes-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  p {
    font-size: 22px;
    color: #444ce0;
  }
  svg {
    margin-right: 12px;
    color: #999;
    &.active {
      color: #444ce0;
    }
    &:last-child {
      margin-right: 0;
    }
  }
  &.full {
    justify-content: center;
    p {
      margin-right: 16px;
      &:last-child {
        margin-right: 0;
      }
    }
  }
} */
</style>
