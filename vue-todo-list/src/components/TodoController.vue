<template>
	<footer class="footer" v-show="todos.length" v-cloak>
		<span class="todo-count">
			<strong>{{remaining}}</strong> {{remaining | pluralize}} left
		</span>
        <ul class="filters">
            <li><a href="#/all" :class="{selected: visivility === 'all'}" >All</a></li>
            <li><a href="#/active" :class="{selected: visivility === 'active'}">Active</a></li>
            <li><a href="#/completed" :class="{selected: visivility === 'completed'}">Completed</a></li>
        </ul>
        <button class="clear-completed" 
            v-show="todos.length > remaining"
			@click="removeCompleted">
            Clear Completed
        </button>

	</footer>
</template>

<script>
export default {
	name: 'TodoController',
	props: {
		todos: Array,
        remaining: Number,
        visivility: String
    },
    filters: {
        pluralize(n){
            return n === 1 ? 'item' : 'items';
        }
    },
    Methods: {
        removeCompleted() {
			this.$emit('removeCompleted');
		}
    }
}
</script>