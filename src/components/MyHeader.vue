<template>
  <div class="todo-header">
    <input type="text" placeholder="Type your tasks here and press enter" @keyup.enter="add" />
  </div>
</template>

<script>
import {nanoid} from 'nanoid'
export default {
  name: 'MyHeader',
  props:['receive'], 
  methods:{
      add(e){
          if (!e.target.value) return alert("You can't leave it as blank")
          const todo = {id: nanoid(), title: e.target.value, completed: false}

          this.receive(todo)
          e.target.value = ''
      }
  }
};
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style> 