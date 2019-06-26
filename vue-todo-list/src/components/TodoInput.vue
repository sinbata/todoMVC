<template>
  <input
    class="new-todo"
    autofocus
    autocomplete="off"
    placeholder="What needs to be done?"
    v-model="newTodo"
    @keypress.enter="onAddTodo"
  >
  <!-- 
    enterを押したらonAddTodo
    newtodoとのv-model
  -->
</template>

<script>
export default {
  name: "TodoInput",
  data() {
    return {
      newTodo: "" //入力欄に空白を入れる（初期化）
    };
  },
  methods: {
    onAddTodo() {
      this.$emit("add-todo", this.newTodo); //$emit  現在のインスタンス上のイベントをトリガ
      this.newTodo = ""; //入力欄に空白を入れる（初期化）
    }
  }
};
</script>

