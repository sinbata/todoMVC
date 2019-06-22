<template>
  <div class="view">
    <input
      type="checkbox"
      class="toggle"
      :value="todo.completed"
      @change="onInput"
      :checked="todo.completed"
    >
    <!-- 左のチェックボックス -->
    <label @dblclick="editTodo">{{todo.title}}</label>
    <!-- todo内容 -->
    <button class="destroy" @click="removeTodo"></button>
    <!-- 削除ボタン -->
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todo: Object
  },
  methods: {
    removeTodo() {
      this.$emit("remove-todo", this.todo);
    },
    onInput() {
      this.$emit("done", this.todo, !this.todo.completed);
    },
    editTodo() {
      this.$emit("edit-todo", this.todo);
    }
  }
};
</script>