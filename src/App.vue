<template> 
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :receive="receive"/>
        <MyList :todos="todos" />
        <MyFooter :todos="todos" :checkAlltodo="checkAlltodo"/>
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from 'pubsub-js';
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: { MyHeader, MyFooter, MyList },
  data(){
    return{
       todos:JSON.parse(localStorage.getItem('todos')) || []
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
    updateTodo(id, title){
      this.todos.forEach((todo)=>{
        if (todo.id === id) todo.title = title
      })
    },
    deleteTodo(_,id){
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id
      }) 
    },
    checkAlltodo(completed){
      this.todos.forEach((todo)=>{
        todo.completed = completed 
      })
    
    }
  },
  watch:{
    todos:{
      deep:true,
      handler(value){
        localStorage.setItem('todos', JSON.stringify(value))
      }
    }
  },
  mounted(){
    this.$bus.$on('checkAlltodo', this.remove)
    //this.$bus.$on('deleteTodo', this.deleteTodo)
    this.$bus.$on('updateTodo', this.updateTodo)
    this.pubsId = pubsub.subscribe('deletTodo', this.deleteTodo);
  },
  beforeDestroy(){
    this.$bus.$off('checkAlltodo')
    this.$bus.$off('updateTodo')
    pubsub.unsubscribe(this.pubsId);
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
  margin-right: 5px;
}
.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid #4591b4;
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
