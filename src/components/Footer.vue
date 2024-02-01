<template>
  <div class="todo-footer">
    <label>
       <input type="checkbox" v-model="isCheckAll"/>
    </label>
    <span>
       <span>Completed{{count}}</span> / Total{{todos.length}}
    </span>
    <button class="btn btn-danger" @click="clearAllCompletedTodos">Delete tasks completed</button>
  </div>
</template>
<script lang="ts">
import { defineComponent, computed } from 'vue';
import {Todo} from '../types/todo'
export default defineComponent({
  name: 'Footer',
  props:{
    todos:{
      type:Array as ()=>Todo [],
      required:true
    },
    checkAll:{
      type:Function,
      required:true
    },
    clearAllCompletedTodos:{
      type:Function,
      required:true
    }
  },
  setup(props){
    const count = computed(()=>{
    return props.todos.reduce((pre, todo, index) => pre + (todo.isCompleted ? 1 : 0), 0)
    })
    const isCheckAll=computed({
      get(){
        return count.value > 0 && props.todos.length === count.value
      },
      set(val){
       props.checkAll(val)
      }
    })
    return {
      count,
      isCheckAll
    }
  },
})
</script>
<style scoped>
/* Footer */
.todo-footer {
    height: 68px;
    line-height: 28px;
    padding-left: 6px;
    margin-top: -6px;
}
.todo-footer label {
    display: inline-block;
    margin-right: 3px;
    cursor: pointer;
}
.todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 2px;
}
.todo-footer button {
    float: right;
    margin-top: 2px;
    margin-right: 2px;
}
</style>