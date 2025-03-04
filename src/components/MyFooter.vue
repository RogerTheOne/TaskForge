<template>
  <div class="todo-footer" v-show="todos.length">
    <label>
      <input type="checkbox" :checked="getTotal === todos.length " @change="checkAll"/>
    </label>
    <span> <span>Completed: {{getTotal}}</span> / All: {{todos.length}} </span>
    <button class="btn btn-danger">Clear Completed Tasks</button>
  </div> 
</template>
  
<script>
export default {
  name: 'MyFooter',
  props:['todos','checkAlltodo'],
  computed:{
    getTotal(){
      return this.todos.reduce((pre,current) => pre + (current.completed? 1:0) ,0)
    }
  },
  methods:{
    checkAll(e){
      this.checkAlltodo(e.target.checked)
    }
    
  }
};
</script>

<style scoped>
/*footer*/
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