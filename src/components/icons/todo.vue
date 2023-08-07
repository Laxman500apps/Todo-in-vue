<script setup>
  import {ref, onMounted, computed, watch} from 'vue' 
  const todos = ref([])
  const name = ref("") 
  const i = ref(0)
  const content = ref("")
  const hideCompleted = ref(false)

// filtering todolist based on completed status
  const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
  })

// add a new todo
function addTodo() {
  if (content.value.trim() === '') {
    alert("enter todo")
  }
  else {

    todos.value.push({
      id : i.value++,
      text : content.value,
      done : false
    })
  }
  content.value = ""
} 

// removing a todo

function removeTodo(index) {
  todos.value.splice(index,1)
}

// update todolist to local storage
  watch(todos, (newval) => {
    localStorage.setItem("todos", JSON.stringify(newval))
  },{deep : true} )

// update name to local storage

  watch(name , (newval) => {
    localStorage.setItem("name", newval)
  },{deep:true})

  
// loading name and todolist on webpade load
  onMounted(() => {
    name.value = localStorage.getItem("name") || ""
    todos.value = JSON.parse(localStorage.getItem('todos'))
  })

</script>

<template>
  <div>
    <h2>
      what's up, <input type="text" class="change" placeholder="YOUR NAME" v-model="name" />
    </h2>
    <div>
      <h2>Add a task</h2>
      <form @submit.prevent="addTodo">
        <input type="text" placeholder="Add your task here" v-model="content" />
        <button type="submit">+</button>
      </form>
    </div>
    <div>
      <h2>Task List</h2>
      <h2 v-for="(todo, index) in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done">
        <span ><input :class="{ done: todo.done }" class = "change" type="text" v-model="todo.text"></span>
        <button @click="removeTodo(index)">x</button>
      </h2>
      <br/>
      <button @click="hideCompleted = !hideCompleted">
          {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
    </div>
  </div>
  
</template>

<style scoped>
input {
  border : none
}
.done {
  text-decoration: line-through;
}
.change {
  font-size: 16px;
}
</style>
