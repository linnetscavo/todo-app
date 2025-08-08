<script setup>
import { ref, computed } from 'vue';
// выдаем всем todo уникальные id
let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const showCongrats = ref(false)
const todos = ref([
  { id: id++, text: 'Изучить HTML', done: true},
  { id: id++, text: 'Изучить JavaScript', done: true },
  { id: id++, text: 'Изучить Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
  ? todos.value.filter((t) => !t.done)
  : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done:false})
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}


function checkDone(todo) {
  if (todo.done) {
    showCongrats.value = true
  }
}

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
      <h1>
        Todo list app
      </h1> 
  </header>

  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Добавить задачу</button>
  </form> 
  
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done"  @change="checkDone(todo)">
      <span :class="{ done: todo.done }">{{ todo.text }}</span> 
      <button @click="removeTodo(todo)">X</button>
    </li>
    <button style="margin-top: 20px;" @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Показать все' : 'Скрыть выполненные' }}
  </button>
  </ul>
  <h1 v-if="showCongrats" @click="showCongrats = false">🎉 Поздравляем!</h1>

</template>

<style scoped>
input[type="checkbox"] {
  accent-color: #42b983;
  width: 18px;
  height: 18px;
  margin-right: 10px;
}

.done {
  text-decoration: line-through;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}
ul{
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin-top: 30px;
  margin-bottom: 10px;
  padding: 0;
  list-style: none;
}
li {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  margin-top: 1rem; 
  margin-bottom: 0.5rem;

}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  background-color: #f7f7f7; 
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); 
  width: 300px; 
  margin: 0 auto;
}
</style>
