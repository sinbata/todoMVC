<template>
	<div class="view">
		<input
			type="checkbox" class="toggle"> <!-- 左のチェックボックス -->
		<label>{{todo.title}}</label> <!-- todo内容 -->
		<button
			class="destroy" 
            @click="removeTodo">
		</button> <!-- 削除ボタン -->
	</div>
</template>

<script>
export default {
	name: 'TodoItem',
	props: {
		todo: Object
    },
    methods:{
        removeTodo(){
            this.$emit('remove-todo', this.todo);
        }
    }
}
</script>