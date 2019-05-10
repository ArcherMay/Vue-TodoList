<template>
     <div class="todo-footer">
        <label>
          <input type="checkbox" v-model="selectAll"/>
        </label>
        <span>
          <span>已完成{{completeNum}}</span> / 全部{{todos.length}}
        </span>
        <button class="btn btn-danger"  @click="clearAllComplete">清除已完成任务</button>
      </div>
</template>
<script>
export default {
    props:{
      todos:Array,
      selectAllTodo:Function
    },
    data() {
      return {
      }
    },
    methods: {
      clearAllComplete(){
        if(window.confirm(`确认删除吗`)){
          this.$emit('FclearAllComplete');
        }
      }
    },
    //如果既要监视一个标签的动作，那么使用computed计算属性
    //如果还需要去通过操作去改变她的值，那么要使用get和set方法实现
    computed:{
      completeNum(){
        return this.todos.reduce((preTotal ,todo)=>preTotal + (todo.complete?1:0),0)
      },
      selectAll:{
        get() {
          return this.completeNum === this.todos.length && this.completeNum >0
        },
        set(value){
          this.selectAllTodo(value)
        }
      }
    }
}
</script>
<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>
