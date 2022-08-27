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


<!-- binding class and only adding to items, that has done =true -->
  <div
  v-for="todo in todos"
    class="card mb-5"
    :class="{ 'has-background-success-light': todo.done }"
  >
  <div class="card-content">
    <div class="content">

      <div class="columns is-mobile is-vcentered"> 
          <!-- if done = true then apply bulma helper class to make text green -->
        <div
          :class="{ 'has-text-success line-through': todo.done }"
          class="column"
        >
          {{todo.content}}
        </div>
        <div class="column is-5 has-text-right">
         <button
            @click="toggleDone(todo.id)"
            :class="todo.done ? 'is-success' : 'is-light'"
            class="button">&check;</button
          >
          <button
            @click="deleteTodo(todo.id)"
            class="button is-danger ml-2">&cross;</button
          >
        </div>

      </div>
    
    
    </div>
  </div>
</div>

</div>
</template>

<script setup>

// imports
import {onMounted, ref} from 'vue';

// imported firebase
import { collection, getDocs, onSnapshot, addDoc, deleteDoc, doc, updateDoc } from 'firebase/firestore';
import {db} from '@/firebase';

// firebase refs

const todosCollectionRef = collection(db, 'todos');


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
  //   done: true 
  // }
    
])

// GET TODOS FROM FIREBASE

//because we use await, we need to use async function on onMounted(). need to add async to onmounted if uncommented, because await
onMounted(  () => {
//xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
// this works, but no real time updates
//  const querySnapshot = await getDocs(collection(db, "todos"));
//   let fbTodos = [];
//   querySnapshot.forEach((doc) => {
//     console.log(doc.id, " => ", doc.data());
//     const todo = {
//       id: doc.id,
//       content: doc.data().content,
//       done: doc.data().done
//     }
//     fbTodos.push(todo);
// });
// todos.value = fbTodos;
// });
//xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

  onSnapshot(todosCollectionRef, (querySnapshot) => {
  const fbTodos = [];
  querySnapshot.forEach((doc) => {
      const todo = {
      id: doc.id,
      content: doc.data().content,
      done: doc.data().done
    }
    fbTodos.push(todo);
  });
  todos.value = fbTodos;
  });
  
});




// add todo
//setting up a ref for the user input to grab. then setting the value to the input value by adding a v-model to the input up above. 
const newTodoContent = ref('')

const addTodo =  () => {
//xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
// // this is for local update. see firebase below
//   const newTodo = {
//     id: uuidv4(), //uuidv4 is a npm package that generates a random id
//     content: newTodoContent.value,
//     done: false
//   }
//   //console.log('newTodo', newTodo)
//   // pushing the newTodo to the todos array (the beginning)
//   todos.value.unshift(newTodo)
//   // clearing the input field
//   newTodoContent.value = ''
//   //focus on the input field
//   focusInputField()
//xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

  addDoc(todosCollectionRef, {
  content: newTodoContent.value,
  done: false
});
newTodoContent.value = ''
focusInputField()
  
}

// delete todo
const deleteTodo = (id) => {
  // //console.log('id', id)
  // // filter through the todos array and return all the todos that don't match the id
  // const updatedTodos = todos.value.filter(todo => todo.id !== id)
  // //console.log('updatedTodos', updatedTodos)
  // // set the todos array to the updatedTodos array
  // todos.value = updatedTodos
  deleteDoc(doc(todosCollectionRef, id))
}


const focusInputField = () => {
   const inputField = document.querySelector('.input')
    inputField.focus()
}

// toggle done
const toggleDone = (id) => {
  // //console.log('id', id)
  // // filter through the todos array and return all the todos that don't match the id
  // const updatedTodos = todos.value.map(todo => {
  //   if (todo.id === id) {
  //     return {
  //       ...todo,
  //       done: !todo.done
  //     }
  //   }
  //   return todo
  // } )
  // //console.log('updatedTodos', updatedTodos)
  // // set the todos array to the updatedTodos array
  // todos.value = updatedTodos
  
  const index = todos.value.findIndex(todo => todo.id === id)
  
  updateDoc(doc(todosCollectionRef, id), {
    done: !todos.value[index].done
  })
  
}

</script>



<style>

@import 'bulma/css/bulma.min.css';

.badass-todo {
  max-width: 400px;
  padding: 20px;
  margin: 0 auto;
}

.line-through{
  text-decoration: line-through;
}

</style>