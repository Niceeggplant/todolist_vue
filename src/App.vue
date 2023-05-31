<template>
  <div id="root">
    <div class="todo-warp">
      <div class="todo-box">
        <el-button type="text" @click="dialogVisible = true">点击打开 Dialog</el-button>

<el-dialog
  title="偏好选择"
  :visible.sync="dialogVisible"
  width="30%"
  :before-close="handleClose">
  <el-form ref="form" :model="sizeForm" label-width="70px" size="mini">
  <el-form-item label="游玩景点">
    <el-select v-model="value1" style="width: 100%;"  multiple placeholder="请选择">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
  </el-form-item>
  <el-form-item label="目的特征">
    <el-select v-model="value2" style="width: 100%;"  multiple placeholder="请选择">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
  </el-form-item>
  <el-form-item label="饮食偏好">
    <el-select v-model="value3" style="width: 100%;"  multiple placeholder="请选择">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
  </el-form-item>

     
        <el-form-item label="景点数">
          <el-input v-model="input" placeholder="请输入想要游玩的景点数目"></el-input>
        </el-form-item>
      
     
 
  <el-form-item label="游玩时间">
    <el-date-picker
     style="width: 100%"
      v-model="value4"
      type="datetimerange"
      start-placeholder="开始日期"
      end-placeholder="结束日期"
      :default-time="['12:00:00']">
    </el-date-picker>
  </el-form-item>
  <el-form-item label="购物需求">
    <el-checkbox-group v-model="sizeForm.type">
      <el-checkbox-button label="商场购物" name="type"></el-checkbox-button>
      <el-checkbox-button label="特产购物" name="type"></el-checkbox-button>
    </el-checkbox-group>
  </el-form-item>
  <el-form-item label="偏好出行">
    <el-radio-group v-model="sizeForm.resource" size="mini">
      <el-radio-button border label="地铁"></el-radio-button>
      <el-radio-button border label="驾车"></el-radio-button>
      <el-radio-button border label="走路"></el-radio-button>
      <el-radio-button border label="骑车"></el-radio-button>
    </el-radio-group>
  </el-form-item>
  
</el-form>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
</el-dialog>
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
      options: [{
          value: '选项1',
          label: '朝天门'
        }, {
          value: '选项2',
          label: '渣滓洞'
        }, {
          value: '选项3',
          label: '人文'
        }, {
          value: '选项4',
          label: '网红打卡'
        }, {
          value: '选项5',
          label: '自然景观'
        }, {
          value: '选项6',
          label: '甜口'
        }, {
          value: '选项7',
          label: '辛辣重口'
        }, {
          value: '选项8',
          label: '素食'
        },
      ],
        value1: [],
        value2: [],
        value3: [],
        value4: [],
      sizeForm: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
      dialogVisible: false,
      todos:JSON.parse(localStorage.getItem('todos')) || [
        {id:'0',title:'看vue.js的书',done:true},
        {id:'1',title:'阅读并理解vue源码',done:false},
        {id:'2',title:'看几篇掘金好文章',done:true},
        {id:'3',title:'学习react技术',done:true},
      ]
    }
  },
  methods: {

    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(()=> {
            done();
          })
          .catch(() => {});
      },
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
