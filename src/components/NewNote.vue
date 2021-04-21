<template>
  <!-- new note -->
  <div class="new-note">
    <label for="">Title</label>
    <input v-model="note.title" type="text">
    <label for="">Description</label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New Note</button>
  </div>
</template>

<!-- чтобы принять note в компоненте: -->
<!-- объявляем пропс  -->
<script>
export default {
  props: {
    note: {
      type: Object,
      required: true
    }
  },
  // так как вызываем в компоненте метод addNote, то должны добавить methods
  // и создать там метод addNote()
  methods: {
    addNote () {
      // создаём эмит, чтобы отправлять данные из компонента родителю
      // первый параметр - название эмита, второй - то, что должно передаваться родителю
      // нам ведь нужно вернуть всю заметку в родительский компонент
      this.$emit('addNote', this.note)
    }
  }
}
</script>

<style lang="scss">
  .new-note {
    text-align: center;
  }
</style>