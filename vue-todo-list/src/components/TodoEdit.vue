<template>
  <input
    id="edit"
    class="edit"
    type="text"
    v-todo-focus="todo == editedTodo"
    :value="todo.title"
    @input="onInput"
    @keypress.enter="onDoneEdit"
    @keyup.esc="onCancelEdit"
    @blur="onCancelEdit"
  >
  <!-- 
    todo == editedTodo => edit中ならfocusをそこに
    enterを押したらonDoneEdit
    escを押したらonCancelEdit
    blur(他のところ押したら)onCancelEdit
  -->
</template>

<script>
export default {
  name: "TodoEdit",
  directives: {
    //カスタムディレクティブ
    ["todo-focus"](element, binding) {
      if (binding.value) {
        element.focus(); //要素にフォーカス(テキスト打ってますってやつ)
      }
    }
  },
  props: {
    todo: Object,
    editedTodo: Object
  },
  data() {
    return {
      editedTitle: null
    };
  },
  mounted() {
    this.editedTitle = this.todo.title; //editedtieにtodo.title
  },
  methods: {
    onInput(event) {
      this.editedTitle = event.target.value; //
    },
    onDoneEdit(event) {
      this.editedTitle = event.target.value; //
      this.$emit("done-edit", this.editedTitle); //編集されたことをemit
    },
    onCancelEdit(event) {
      event.target.value = this.todo.title; //event.target.valueにtodotileを (元のtodo)
      this.$emit("cancel-edit"); //cancel-editのemit
    }
  }
};
</script>
