<template>
  <div id="root">
    <div class="todo-warp">
      <div class="todo-box">
        <MyHeader @addTodo="addTodo"/>
        <MyList :todos="todos"/>
        <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from './components/MyHeader'
import MyList from './components/MyList'
import MyFooter from './components/MyFooter.vue'

export default {
  name:'App',
  components:{MyHeader,MyList,MyFooter},
  data() {
    return {
      todos:JSON.parse(localStorage.getItem('todos')) || [
        {id:'0',title:'看vue.js的书',done:true},
        {id:'1',title:'阅读并理解vue源码',done:false},
        {id:'2',title:'看几篇掘金好文章',done:true},
        {id:'3',title:'学习react技术',done:true},
      ]
    }
  },
  methods: {
    //添加
    addTodo(todoObj){
      this.todos.unshift(todoObj)
    },

    //勾选或者取消勾选
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.done = !todo.done
      })
    },

    //删除
    deleteTodo(id){
      this.todos = this.todos.filter( todo => todo.id !== id )
    },
    //全选or取消全选
    checkAllTodo(done){
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    
    // 编辑
    updateTodo(id,title){
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.title = title
      })
    },
    
    //清除所有已经完成的任务
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=>{
        return !todo.done
      })
    }
  },
  
  watch: {
    todos:{
      deep:true,
      handler(value){
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  },
  
  mounted() {
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
    this.$bus.$on('deleteTodo',this.deleteTodo)
  },
  beforeDestroy() {
    this.$bus.$off('checkTodo')
    this.$bus.$off('updateTodo')
    this.$bus.$off('deleteTodo')
  },
}
</script>

<style>

body {
  background: #fff;
}

.todo-warp {
  width: 600px;
  margin: 0 auto;
}
.todo-warp .todo-box {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
