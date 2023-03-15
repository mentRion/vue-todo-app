<!-- ---------------------------------------------------------------- -->
<!-- Scripts -->
<!-- ---------------------------------------------------------------- -->
<script setup>

// ref = obtain direct reference
import { ref } from 'vue';

let todoList = ref([]);
let todo = ref(null);

// adding new todo in list
const addTodo = () => {
  if (validateTodo(todo.value)) {
    let id = todoList.value.length;
    todoList.value.push({
      id: ++id,
      todo: todo.value,
      is_done: false
    })
    todo.value = null
  }
}

// checks if the value is already exist in the todolist
// or to prevent duplication in todolist
const validateTodo = (new_todo) => {
  let index = todoList.value.findIndex((t) => t.todo === new_todo);
  if (index !== -1) {
    alert("Todo already exist!");
    return false;
  } else {
    return true;
  }

}

// Select todolist to edit
// returns value if a todo is selected
let selectedTodo = ref(null);
const selectTodo = (row) => {
  selectedTodo.value = row;
  todo.value = row.todo;
}

//update specific todo
const updateTodo = () => {
  if (validateTodo(todo.value)) {
    let index = todoList.value.findIndex((t) => t.id === selectedTodo.value.id);
    index !== -1 && (todoList.value[index].todo = todo.value);
    todo.value = selectedTodo.value = null;
  }
}

//set a todo as done
const markAsDone = (row) => {
  if (confirm(`Are you sure you want to mark ${row.todo} as done?`)) {
    let index = todoList.value.findIndex((t) => t.id === row.id);
    index !== -1 && (todoList.value[index].is_done = true);
  }
}

//removes todo from the list
const removeTodo = (row) => {
  if (confirm(`Are you sure you want to delete ${row.todo}?`)) {
    let index = todoList.value.findIndex((t) => t.id === row.id);
    index !== -1 && todoList.value.splice(index, 1);
  }
}

</script>

<!-- ---------------------------------------------------------------- -->
<!-- Template -->
<!-- ---------------------------------------------------------------- -->
<template>
  <div class="d-flex justify-content-center container">
    <div class="w-75">

      <!-- if to do is not selected addTodo() else updateTodo() -->
      <form @submit.prevent="!selectedTodo ? addTodo() : updateTodo()">
        <div class="mb-3 mt-5">
          <label for="todo">What's your todo?</label>
          <div class="d-flex">
            <input v-model="todo" type="text" class="form-control" id="todo" placeholder="eg. Create todo application">
            <!-- add todo button -->
            <!-- disappears when you haven't selected a todo -->
            <button v-if="!selectedTodo" class="btn btn-primary ms-3"><i class="bi bi-plus-circle-fill"></i></button>
            <!-- update todo button -->
            <!-- appears when a todo is selected -->
            <button v-else class="btn btn-primary ms-3"><i class="bi bi-pencil-square"></i></button>
          </div>
        </div>
      </form>

      <!-- list of todo -->
      <div @click="selectTodo(row)" v-for="row in todoList" :key="row.id" class="toast show mt-2 w-100">
        <div class="toast-header">
          <strong class="me-auto">{{ row.todo }}</strong>
          <!-- check of todolist -->
          <button :class="`btn btn-${row.is_done ? 'success' : 'outline-secondary'}`" @click.stop="markAsDone(row)">
            <i class="bi bi-check-lg"></i></button>
          <!-- delete/or remove of todolist -->
          <button class="btn btn-danger btn-sm ms-1" @click.stop="removeTodo(row)">
          <i class="bi bi-trash-fill"></i></button>
        </div>
      </div>

    </div>
</div>
</template>