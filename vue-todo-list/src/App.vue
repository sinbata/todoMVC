<template>
	<section id="app" class="todoapp">
		<header class="header">
			<h1>todos</h1>
			<todo-input
				class="new-todo" autofocus autocomplete="off"
				placeholder="What needs to be done?"
				@add-todo="addTodo"> 
			</todo-input>
      <!-- @add-todo — TodoinputのaddTodo() methodから
      v-on : 省略[@] 要素にイベントリスナをアタッチ
      -->
		</header>
		<todo-list
			:todos="todos"
			:filtered-todos="filteredTodos"
      @remove-todo="removeTodo">
		</todo-list>
    <!-- 
      :todos -> v-bind:todos  
      :filtered-todos -> v-model:filtered-todos
    -->
	</section>
</template>

<script>
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';

export default {
	name: 'app',
	components: {
		TodoInput,
		TodoList
	},
	data() {
		return {
			todos: [],
			uid: 0
		}
	},
	computed: {
		filteredTodos() { // computed Vueインスタンスに組み込まれる算出プロパティ
			return this.todos;
		}
	},
	methods: {
		addTodo(todoTitle) {
			const newTodo = todoTitle && todoTitle.trim();
			if (!newTodo) {
				return;
			}
			this.todos.push({
				id: this.uid++,
				title: newTodo,
				completed: false
			});
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((item) => item !== todo);
		}
	}
}
</script>

<style>
@import url("https://unpkg.com/todomvc-app-css@2.2.0/index.css");
</style>