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
    todoStorage.uid = todos.length; //uidにlength
    return todos;
  },
  save(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos)); //stringilyで文字列にして書き込み
  }
};
const filters = {
  //filterの関数
  all(todos) {
    return todos; //全てのtodo
  },
  active(todos) {
    return todos.filter(todo => !todo.completed); //completedではない(active)を返す
  },
  completed(todos) {
    return todos.filter(todo => {
      return todo.completed; //completedを返す
    });
  }
};
const getVisibility = () => {
  return window.location.hash.replace(/#\/?/, " ").trim(); //正規化して返す
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
      todos: todoStorage.fetch(), // todostrageにfetchしたものを入れる
      visibility: getVisibility() || "all" //初期をallに
    };
  },
  computed: {
    // computed Vueインスタンスに組み込まれる算出プロパティ
    filteredTodos() {
      return filters[this.visibility](this.todos); // filterを使った判別
    },
    remaining() {
      const todos = filters.active(this.todos); //activeでのfilterしたものをtodosに代入
      return todos.length; //todoの量を返す
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
    //Vue インスタンス上でのひとつの式または算出関数 (computed function) の変更を監視します。
    todos: {
      handler(todos) {
        todoStorage.save(todos); //変更されたらローカルに保存
      },
      deep: true
    }
  },
  mounted() {
    //新たに作成される vm.$el によって置き換えられる el に対して、インスタンスがマウントされたちょうど後に呼ばれます。
    window.addEventListener("hashchange", this.onHashChange);
  },
  methods: {
    onAddTodo(todoTitle) {
      const newTodo = todoTitle && todoTitle.trim(); //newtodoが空でなければString.trim()メソッドで両端の空白を除く
      if (!newTodo) {
        return;
      }
      this.todos.push({
        id: todoStorage.uid++, //idを固有のものに
        title: newTodo, //タイトルをnewtodoに
        completed: false
      });
      todoStorage.save(this.todos); //保存
    },
    onRemoveTodo(todo) {
      this.todos = this.todos.filter(item => item !== todo); //itemとtodoが違かったらそれをfilterして代入
      //todoStorage.save(this.todos);
    },
    onDone(todo, completed) {
      todo.completed = completed; //completed
    },
    onHashChange() {
      //console.log(JSON.stringify(visibility));
      this.visibility = getVisibility(); //all-active-completedの状態変化を取得
    },
    onAllDone(done) {
      this.allDone = done; //全てdoneにする
    },
    onRemoveCompleted() {
      //completedの削除
      this.todos = filters.active(this.todos); // activeのものを全体に書き換え
    }
  }
};
</script>

<style>
@import "./index.css";
</style>