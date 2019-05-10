<template>
<div class="todo-container">
    <div class="todo-wrap">
        <TodoHeader :AddTodos='AddTodos'/>
        <TodoList :todos='todos' :deleteTodo='deleteTodo'/>
        <TodoFooter :todos='todos'  :selectAllTodo='selectAllTodo' :FclearAllComplete='FclearAllComplete'/>
    </div>
</div>
</template>
<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
export default {
    data() {
        return {
            todos:[
                {title:'Vue',complete:true},
                {title:'node',complete:false},
                {title:'html',complete:false},
                {title:'css',complete:false}
            ]
        }
    },
    components:{
        TodoHeader,
        TodoList,
        TodoFooter
    },
    watch: {
        
    },
    methods: {
        AddTodos(todoObject) {
        this.todos.unshift(todoObject)
        },
        deleteTodo(index){
            this.todos.splice(index,1)
        },
        selectAllTodo(isAll){
            this.todos.forEach(todo => {
                todo.complete = isAll
            });
        },
        FclearAllComplete(){
            this.todos = this.todos.filter(todo => !todo.complete) //过滤出所有complete 不为true的，留下true的
        }
    }
}
</script>
<style scoped>
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
