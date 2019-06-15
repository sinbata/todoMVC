<template>
	<input
		class="new-todo" autofocus autocomplete="off"
		placeholder="What needs to be done?"
		v-model="newTodo"
		@keypress.enter="addTodo">
        <!-- 
            v-model : 省略[:]form の input 要素またはコンポーネント上に双方向バインディングを作成
            v-on : 省略[@] 要素にイベントリスナをアタッチ
                        .{keyCode | keyAlias} - 指定したキーが押された時のみトリガされる
         -->
</template>

<script>
export default {
	name: 'TodoInput',
	data() {
		return {
			newTodo: '' //入力欄に空白を入れる（初期化）
		};
	},
	methods: {
		addTodo() {
			this.$emit('add-todo', this.newTodo); //$emit  現在のインスタンス上のイベントをトリガ
			this.newTodo = ''; //入力欄に空白を入れる（初期化）
        }
	}
}
</script>

