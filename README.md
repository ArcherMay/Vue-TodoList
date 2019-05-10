# vue_demo

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 项目结构

1、入口：main.js

2、父组件； App.vue

​		子组件：todoheader  

​						todoList 

​						todoFooter 

​						todoItem



## 实现的功能

1、enter键绑定添加事件

2、计算属性监控  选中和是否选中 以及选择的数量

3、计算属性的get和set方法的使用

4、单个删除和部分删除和全部删除功能，父组件定义方法，子组件通过props和事件绑定的方式获取父组件的方法

5、鼠标移动改变item的样式

## 需要重要关注的点
1、todoFooter的计算属性的使用,v-model绑定的属性，在计算属性中这么定义

```html
<div class="todo-footer">
        <label>
          <input type="checkbox" v-model="selectAll"/>
        </label>
        <span>
          <span>已完成{{completeNum}}</span> / 全部{{todos.length}}
        </span>
        <button class="btn btn-danger"  @click="clearAllComplete">清除已完成任务</button>
      </div>
```



``` 
 	computed:{
 	 //普通的计算属性通过函数的形式定义
      completeNum(){
        return this.todos.reduce((preTotal ,todo)=>preTotal + (todo.complete?1:0),0)
      },
      //通过v-model方法使用的计算属性如下定义
      selectAll:{
        get() {
          return this.completeNum === this.todos.length && this.completeNum >0
        },
        set(value){
          this.selectAllTodo(value)
        }
      }
    }
```

2、@mouseenter 与mouseleave 方法的使用

```html
<li @mouseenter="handleEnter(true)" 			
    @mouseleave="handleEnter(false)" 
    :style="{background:bgColor}">
     <label>
         <input type="checkbox" v-model="todo.complete"/>
          <span>{{todo.title}}</span>
      </label>
      <button class="btn btn-danger" v-show="isShow" @click="deleteItem">删除</button>
  </li>
```

mouseenter 进入标签时的函数，可以设置style样式

mouseleave 离开时的函数

mouseenter 与mouseover的区别：

二者的本质区别在于，mouseenter不会冒泡，简单的说就是他不会被它本身的子元素的状态影响到，但是mouseover就会被子元素的状态影响到，在触发子元素的状态时，会冒泡到父元素

共同点：当二者都没有子元素时，二者的行为是一致的。

3、父子组件传递函数和数据

​        操作数据 方法（增删改）的定义位置原则：数据在哪定义的，操作的方法就要在哪定义。

*其他子组件如果需要用到*，可以通过父子组件之间的传递实现

比如：

``` vue
父组件App.vue
html:
<div class="todo-wrap">
        <TodoHeader :AddTodos='AddTodos'/>
        <TodoList :todos='todos' :deleteTodo='deleteTodo'/>
        <TodoFooter :todos='todos'  :selectAllTodo='selectAllTodo'/>
    </div>

javascript:
data() {
        return {
            todos:[
                {title:'Vue',complete:true},
                {title:'node',complete:false},
                {title:'html',complete:false},
                {title:'css',complete:false}
            ]
        }
    }
components:{
        TodoHeader,
        TodoList,
        TodoFooter
    }
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
            this.todos = this.todos.filter(todo => !todo.complete) //过滤出所有complete 不为				true的，留下true的
        }
    }
```

```vue
子组件 TodoHeader 往todos里面新增数据时，只能通过父组件传递过来的AddTodos 去实现，具体的添加动作还是在父组件App.vue中实现
<div class="todo-header">
        <input type="text" 
        placeholder="请输入你的任务名称，按回车键确认" 
        v-model="inputTitle" 
        @keyup.enter="Add"/>
      </div>
  子组件需要通过props属性将父组件的数据和方法引入
  props:{
    todos:Array,
    AddTodos:Function
  }
   然后在子组件自己的Add方法中调用父组件的添加方法
    methods: {
     Add() {
       const todoItem = {
         title:this.inputTitle,
         complete:false
       }
      this.AddTodos(todoItem)
      this.inputTitle =''
     } 

```

