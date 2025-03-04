<template> 
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :receive="receive"/>
        <MyList :todos="todos" :remove="remove" :deleteTodo="deleteTodo"/>
        <MyFooter :todos="todos" :checkAlltodo="checkAlltodo"/>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: { MyHeader, MyFooter, MyList },
  data(){
    return{
       todos:[
         {id:'001',title:'Go Bathing', completed:'true' },
         {id:'002',title:'Go Sleeping', completed:'true' },
         {id:'003',title:'Go Shopping', completed:'true' },
       ]
    }
  },
  methods:{
    receive(todo){
      this.todos.unshift(todo)
    },
    remove(id){
      this.todos.forEach((todo)=>{
        if (todo.id === id) todo.completed = !todo.completed
      })
    },
    deleteTodo(id){
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id
      })
    },
    checkAlltodo(completed){
      this.todos.forEach((todo)=>{
        todo.completed = completed 
      })
    
    }
  }
};
</script>

<style>
/*base*/
body {
  background: #fff;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn:focus {
  outline: none;
}
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
