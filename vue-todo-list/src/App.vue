<template>
  <section id="app" class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <todo-input
        class="new-todo"
        autofocus
        autocomplete="off"
        placeholder="What needs to be done?"
        @add-todo="onAddTodo"
      ></todo-input>
      <!-- @add-todo — TodoinputのaddTodo() methodから
      v-on : 省略[@] 要素にイベントリスナをアタッチ
      -->
    </header>
    <todo-list
      :todos="todos"
      :filtered-todos="filteredTodos"
      @remove-todo="onRemoveTodo"
      @done="onDone"
      :allDone="allDone"
      @allDone="onAllDone"
    ></todo-list>
    <!-- 
      :todos -> v-bind:todos  
      :filtered-todos -> v-model:filtered-todos
    -->
    <todo-controller
      :todos="todos"
      :remaining="remaining"
      :visivility="visibility"
      @removeCompleted="onRemoveCompleted"
    ></todo-controller>
  </section>
</template>

<script>
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";
import TodoController from "./components/TodoController.vue";

const STORAGE_KEY = "todos-vuejs-2.6";
const todoStorage = {
  fetch() {
    const todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"); //localStorage 項目の読み込み -> json化
    todos.forEach(function(todo, index) {
      //それぞれのidにインデックスを代入
      todo.id = index;
    });
    todoStorage.uid = todos.length; //uidにlength
    return todos;
  },
  save(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos)); //stringilyで文字列にして書き込み
  }
};
const filters = {
  all(todos) {
    return todos;
  },
  active(todos) {
    return todos.filter(todo => !todo.completed);
  },
  completed(todos) {
    return todos.filter(todo => {
      return todo.completed;
    });
  }
};
const getVisibility = () => {
  return window.location.hash.replace(/#\/?/, " ").trim();
};
//filter() メソッドは、引数として与えられたテスト関数を各配列要素に対して実行し、それに合格したすべての配列要素からなる新しい配列を生成します。
export default {
  name: "app",
  components: {
    TodoInput,
    TodoList,
    TodoController
  },
  data() {
    return {
      todos: todoStorage.fetch(),
      visibility: getVisibility() || "all"
    };
  },
  computed: {
    filteredTodos() {
      // computed Vueインスタンスに組み込まれる算出プロパティ
      //return this.todos;
      //console.log("1"+this.visibility);
      return filters[this.visibility](this.todos);
    },
    remaining() {
      const todos = filters.active(this.todos);
      return todos.length;
    },
    allDone: {
      get() {
        return this.remaining === 0;
      }, //remainingを0にしつつ返す
      set(value) {
        this.todos.forEach(todo => (todo.completed = value));
      } //チェックボックスから渡された引数値(value)を、リストデータ(todos)のすべての項目のプロパティ(completed)に定めます。
    }
  },
  watch: {
    todos: {
      handler(todos) {
        todoStorage.save(todos);
      },
      deep: true
    }
  },
  mounted() {
    window.addEventListener("hashchange", this.onHashChange);
  },
  methods: {
    onAddTodo(todoTitle) {
      const newTodo = todoTitle && todoTitle.trim();
      if (!newTodo) {
        return;
      }
      this.todos.push({
        id: todoStorage.uid++,
        title: newTodo,
        completed: false
      });
      todoStorage.save(this.todos);
    },
    onRemoveTodo(todo) {
      this.todos = this.todos.filter(item => item !== todo);
      //todoStorage.save(this.todos);
    },
    onDone(todo, completed) {
      todo.completed = completed;
    },
    onHashChange() {
      //console.log(JSON.stringify(visibility));
      this.visibility = getVisibility();
    },
    onAllDone(done) {
      this.allDone = done;
    },
    onRemoveCompleted() {
      this.todos = filters.active(this.todos);
    }
  }
};
</script>

<style>
@import "./index.css";
</style>