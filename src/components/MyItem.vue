<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.completed" @change="handleCheck(todo.id)"/>
      <span v-show="!todo.isEdit">{{todo.title}}</span>
      <input v-show="todo.isEdit" type="text" :value="todo.title" @blur="handleBlur(todo, $event)" ref="inputTitle">

    </label>
     
    <button class="btn btn-danger" @click="handleDelete(todo.id)">Delete</button>
    <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">Edit</button>
  </li>
</template>
  
<script>
import pubsub from 'pubsub-js';
export default {
  name: 'MyItem',
  props:['todo'],
  methods:{
    handleCheck(id){
      //this.remove(id)
      this.$bus.$emit('checkAlltodo', id)
    },
    handleDelete(id){
      alert(confirm("Do you really want to delete it?"))
      //this.deleteTodo(id)
      //this.$bus.$emit('deleteTodo', id)
      pubsub.publish('deleteTodo', id); 
    },
    handleEdit(){
      if (todo.hasOwnProperty('isEdit')){
        todo.isEdit = true
      }else{
        this.$set(todo, 'isEdit', true)
      }
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
      
    },
    handleBlur(todo, e){
      todo.isEdit = false
      if (!e.target.value.trim()) return alert('you cannot leave this as blank')
      this.$bus.$emit('updateTodo', todo.id, e.target.value)
      

    }

  },
};
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

li button {
  float: right;
  display:none; 
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>