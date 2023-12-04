<script setup>
import { ref, onMounted, computed, watch} from 'vue'
import Tasks from './components/Tasks.vue'

const input_content = ref('')

const todos = ref([])

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '') {
    return
  }

  todos.value.push({
    content: input_content.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_content.value = ''
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t=> t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>

  <main class="app">

    <div class="app-box">
      <!-- form to create todo -->
      <section class="create-todo">

        <h3>Create a ToDo</h3>

        <form @submit.prevent="addTodo">
          <h4>What's on your todo list?</h4>
          <input 
            type="text" 
            placeholder="e.g. make a video"
            v-model="input_content"
          />
          <button 
            type="submit"
          >add task</button>
        </form>

      </section>

      <!-- todo lists -->
      <Tasks title="Todo list" :todos="todos" :done="false" :func="removeTodo"/>
      <Tasks title="Done" :todos="todos" :done="true" :func="removeTodo"/>
    
    </div>

  </main>

</template>