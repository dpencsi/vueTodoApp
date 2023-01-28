<script setup>
import { ref, onMounted, watch } from "vue";

const todoInput = ref('');
const todos = ref([]);

const addTodo = () => {

  if(todoInput.value){
    todos.value.push(todoInput.value);
    todoInput.value = '';
  }

};

const removeTodo = todo => {
  todos.value = todos.value.filter(t => (t !== todo));
};

const removeAllTodos = () => {
  todos.value.length = 0;
};

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, {deep: true});

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || [];
});

</script>

<template>
  <h1>Todo app</h1>

<form @submit.prevent="addTodo">
  <input type="text" v-model="todoInput">
  <button>+</button>
</form>

<hr>

<div class="todos">
  <div class="todo_item" v-for="todo in todos">
    <p>{{ todo }}</p>
    <button @click="removeTodo(todo)">X</button>
  </div>
</div>

<hr>

<div class="filter">
  <p>You have {{ todos.length }} peding tasks</p>
  <button @click="removeAllTodos">Clear All</button>
</div>
</template>

<style scoped>

.todo_item p, .filter p{
  display: inline;
}

button {
  margin-left: 10px;
}

.todo_item {
  margin-bottom: 10px;
}

.todo_item button{
  background-color: #c93d3d;
}

.todo_item button:hover{
  border-color: #b11111;
  background-color: #b11111;
}

</style>