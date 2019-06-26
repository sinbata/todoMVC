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
    <label @dblclick="onEditTodo" class="todoLabel">{{todo.title}}</label>
    <!-- todo内容 -->
    <button class="destroy" @click="onRemoveTodo"></button>
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
    onRemoveTodo() {
      this.$emit("remove-todo", this.todo);
    },
    onInput() {
      this.$emit("done", this.todo, !this.todo.completed);
    },
    onEditTodo() {
      this.$emit("edit-todo", this.todo);
    }
  }
};
</script>

