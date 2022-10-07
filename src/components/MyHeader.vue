<template>
	<div class="todo-header">
    <el-input size="small" placeholder="请输入你的任务名称并按回车键确认" v-model="title" @change="addTodo"/>
  </div>
</template>

<script>
	import {nanoid} from 'nanoid'
	export default {
		name:'MyHeader',
		data() {
			return {
				title:''
			}
		},
		methods: {
      addTodo(){
				if(!this.title.trim()){
          return alert('输入不能为空')
        }
				const todoObj = {
          id:nanoid(),
          title:this.title,
          done:false
        }
				this.$emit('addTodo',todoObj)
				this.title = ''
			}
		},
	}
</script>

<style scoped>

</style>