<script setup>
const props = defineProps(['title', 'todos', 'done', 'func'])
</script>

<template>
    <section class="todo-list"> 
        <h4>{{title}}</h4>

          <div 
            v-for="(todo, index) in todos" 
            v-show="todo.done == done"
            :key="index" 
            :class="`todo-item ${todo.done && 'done'}`"
          >

            <div>
              <input type="checkbox" v-model="todo.done"/>
              <input 
                type="text"
                v-model="todo.content"
              />
            </div>

            <button
              @click=func(todo)
            >delete</button>

          </div>

      </section>
      
</template>