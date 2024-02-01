<template>
  <h1 class="app-title">To-do-vue-App</h1>
  <div class="todo-container">
    <div class="todo-wrap">  
      <Header :addTodo="addTodo" />
      <List 
      :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo"/>
      <Footer 
      :todos="todos" :checkAll="checkAll" :clearAllCompletedTodos="clearAllCompletedTodos"/>
    </div>
  </div>
  </template>
  <script lang="ts">
  import { defineComponent, reactive, onMounted, toRefs, watch } from 'vue';
  import Header from './components/Header.vue';
  import List from './components/List.vue';
  import Footer from './components/Footer.vue';
  import {Todo} from './types/todo'
  import {saveTodos, readTodos} from './utils/localStorageUtils'

  export default defineComponent({
    name: 'App',
    components: {
      Header,
      List,
      Footer
    },

    setup() {
      /*
      const state = reactive<{todos: Todo []}>({
        todos: [
          {id:1, title: 'Read a short story', isCompleted:false
          },
          {id:2, title: 'Run', isCompleted:true
          },
          {id:3, title: 'Work', isCompleted:false
          },
        ]
      })
      */
      
      const state = reactive<{todos: Todo[]}> ({
        todos: [],
      })

      onMounted(()=>{
        setTimeout(()=>{
          state.todos = readTodos()
        }, 1000)
      })
      const addTodo =(todo: Todo)=> {
        state.todos.unshift(todo)
      }

      const deleteTodo = (index: number) => {
        state.todos.splice(index, 1)
      }

      const updateTodo=(todo: Todo, isCompleted: boolean)=>{
        todo.isCompleted = isCompleted
        console.log(todo)
      }

      const checkAll = (isCompleted:boolean) => {
          state.todos.forEach(todo=>{
            todo.isCompleted = isCompleted
          })
      }

      const clearAllCompletedTodos = () => {
        state.todos = state.todos.filter((todo)=>!todo.isCompleted)
      }
      watch(()=>state.todos, saveTodos, {deep: true})

      return {
        ...toRefs(state),
        addTodo,
        deleteTodo,
        updateTodo,
        checkAll,
        clearAllCompletedTodos
      }
    }
  })
  </script>
  <style scoped>
  .app-title {
    font-size: 16px;
    text-align: center;
    margin: 16px auto;
  }
  .todo-container {
      width: 278px;
      margin: 0 auto;
  }
  .todo-container .todo-wrap {
      padding: 6px;
      border: 1px solid #ddd;
      border-radius: 5px;
  }
  </style>