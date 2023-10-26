<script setup>
import { ref, computed } from 'vue'

let id = 0

const newName = ref('')
const newYear = ref('WS23/24')
const newNumber = ref(1)
const hideCompleted = ref(false)
const todos = ref([])

const filteredTodos = computed(() => {
  return hideCompleted.value
      ? todos.value.filter((t) => !t.done)
      : todos.value
})

function addTodo() {
  if (newName.value === '') {
    console.log("please input the name of course!")
    return;
  }
  todos.value.push({ id: id++, year:newYear.value, course: newName.value, number: newNumber.value, done: false })
  newName.value = ''
  newNumber.value = 1
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
<!--    TODO: auto generate it with method, maybe el-select-->
    <select v-model="newYear">
      <option value="WS23/24">WS23/24</option>
      <option value="SS24">SS24</option>
      <option value="WS24/25">WS24/25</option>
      <option value="SS25">SS25</option>
      <option value="WS25/26">WS25/26</option>
    </select>
    <input v-model="newName" placeholder="Vorlesung">
    <select v-model="newNumber">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
    </select>
    <button>Submit</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">[<b id="highlight">{{todo.year}}</b>]<b id="highlight">{{todo.course}}</b> grouping with <b id="highlight">{{todo.number}}</b> person(s).</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
#highlight {
  color : darkcyan
}
</style>