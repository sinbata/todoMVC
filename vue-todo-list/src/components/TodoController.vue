<template>
  <footer class="footer" v-show="todos.length" v-cloak>
    <span class="todo-count">
      <strong>{{remaining}}</strong>
      {{remaining | pluralize}} left
      <!-- 要素数 
      strong　強調
      {{remaining | pluralize}} mustache 展開 フィルタ関数は常に式の値(前のチェーンの結果)を第一引数として受け取る
      remainingを引数としたpluralize関数の実行。
      -->
    </span>
    <ul class="filters">
      <li v-for="item in items" :key="item.name">
        <a
          v-bind:href="'#' + item.name"
          v-bind:class="{selected: visivility === item.name}"
        >{{ item.display }}</a>
      </li>
    </ul>
    <button
      class="clear-completed"
      v-show="todos.length > remaining"
      @click="onRemoveCompleted"
    >Clear Completed</button>
    <!-- @clickでonRemoveCompleted起動　
    条件的に要素を表示するためのオプション 要素は常に描画されて DOM に維持する＝＞remainingより要素数が大きかったら => 何か実行されているものがあるのか
    -->
  </footer>
</template>

<script>
export default {
  name: "TodoController",
  props: {
    todos: Array,
    remaining: Number,
    visivility: String
  },
  data() {
    return {
      items: [
        { name: "all", display: "ALl" },
        { name: "active", display: "Active" },
        {
          name: "completed",
          display: "Completed"
        }
      ]
    };
  },
  filters: {
    pluralize(n) {
      return n === 1 ? "item" : "items"; //１だったらitem,それ以外はitems(末尾)
    }
  },
  methods: {
    onRemoveCompleted() {
      this.$emit("removeCompleted"); //completedのものを削除 emit
    }
  }
};
</script>