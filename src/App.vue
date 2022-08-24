<template>

  <div class="badass-todo">
    <div class="title has-text-centered">
      Badass Todo
    </div>
    
    <!-- adding v-model to the input bings it to the ref -->
    <form
      @submit.prevent="addTodo"
    >
      <div class="field is-grouped mb-5">
        <p class="control is-expanded">
          <input
            v-model="newTodoContent" 
            class="input"
            placeholder="Add a todo"
            type="text"
          >
        </p>
        <p class="control">
         <button
            :disabled="!newTodoContent"
            class="button is-info">Add
           </button>
        </p>
      </div>     
    </form>


  <div
  v-for="todo in todos"
    class="card mb-5"
  >
  <div class="card-content">
    <div class="content">
      <div class="columns is-mobile is-vcentered"> 
          {{todo.content}}
        <div class="column">
        </div>
        <div class="column is-5 has-text-right">
          <button class="button is-light">&check;</button>
          <button class="button is-danger ml-2">&cross;</button>
        </div>

      </div>
    
    
    </div>
  </div>
</div>

</div>
</template>

<script setup>

// imports
import {ref} from 'vue';
// imported uuid with npm temporarily, before hooking it up to firebase
import { v4 as uuidv4 } from 'uuid';

//todos

const todos = ref([
  // {
  //   id: 'id1',
  //   content: 'Shave my butt',
  //   done: false
  // },
  // { 
  //   id: 'id2', 
  //   content: 'Wash my butt', 
  //   done: false }
    
])


// add todo

//setting up a ref for the user input to grab. then setting the value to the input value by adding a v-model to the input up above. 
const newTodoContent = ref('')

const addTodo = () => {
  const newTodo = {
    id: uuidv4(), //uuidv4 is a npm package that generates a random id
    content: newTodoContent.value,
    done: false
  }
  //console.log('newTodo', newTodo)
  // pushing the newTodo to the todos array (the beginning)
  todos.value.unshift(newTodo)
  // clearing the input field
  newTodoContent.value = ''
  //focus on the input field
  focusInputField()
  
  
}

const focusInputField = () => {
   const inputField = document.querySelector('.input')
    inputField.focus()
}

</script>

<style>

@import 'bulma/css/bulma.min.css';

.badass-todo {
  max-width: 400px;
  padding: 20px;
  margin: 0 auto;
}


</style>