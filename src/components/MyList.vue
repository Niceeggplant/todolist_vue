<template>
	<ul class="todo-main">
		<MyItem 
			v-for="todoObj in todos"
			:key="todoObj.id" 
			:todo="todoObj" 
		/>
	</ul>
</template>

<script>
	import MyItem from './MyItem'

	export default {
		name:'MyList',
		components:{MyItem},
		props:['todos']
	}
</script>

<style scoped>
	/*main*/
	.todo-main {
		margin: 0px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding: 0px;
	}

</style>