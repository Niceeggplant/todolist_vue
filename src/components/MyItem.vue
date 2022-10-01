<template>
	<li>
		<label>
			<input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
      <span v-show="!todo.isEdit">{{todo.title}}</span>
      <input
          type="text"
          v-show="todo.isEdit"
          :value="todo.title"
          @blur="handleBlur(todo,$event)"
          ref="inputTitle"
      >
		</label>
    <el-button size="mini" type="danger" @click="handleDelete(todo.id)" icon="el-icon-delete" circle></el-button>
    <el-button size="mini"   @click="handleEdit(todo)" v-show="!todo.isEdit" type="primary" icon="el-icon-edit" circle></el-button>

  </li>
</template>

<script>
	export default {
		name:'MyItem',
		props:['todo'],
		methods: {
			//勾选or取消勾选
			handleCheck(id){
				this.$bus.$emit('checkTodo',id)
			},
			//删除
			handleDelete(id){
				if(confirm('确定删除吗？')){
					this.$bus.$emit('deleteTodo',id)
				}
			},
      //编辑
      handleEdit(todo){
        // eslint-disable-next-line no-prototype-builtins
        if(todo.hasOwnProperty('isEdit')){
          todo.isEdit = true
        }else{
          this.$set(todo,'isEdit',true)
          console.log(todo)
        }
        this.$nextTick(function(){
          this.$refs.inputTitle.focus()
        })
      },

      //失去焦点回调（真正执行修改逻辑）
      handleBlur(todo,e){
        todo.isEdit = false
        if(!e.target.value.trim()) return alert('输入不能为空！')
        this.$bus.$emit('updateTodo',todo.id,e.target.value)
      }

    }
	}
</script>

<style scoped>
	/*item*/
	li {
		list-style: none;
		height: 36px;
		line-height: 36px;
		padding: 0 5px;
		border-bottom: 1px solid #ddd;
	}

	li label {
		float: left;
		cursor: pointer;
	}

	li label li input {
		vertical-align: middle;
		margin-right: 6px;
		position: relative;
		top: -1px;
	}

	li:before {
		content: initial;
	}

	li:last-child {
		border-bottom: none;
	}

	li:hover{
		background-color: #ddd;
	}
	
	li:hover button{
		display: block;
	}
  li ::v-deep .el-button {
    float: right;
    margin: 3px;
  }
</style>