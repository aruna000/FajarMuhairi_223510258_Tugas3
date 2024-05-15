<template>
  <link
    rel="stylesheet"
    href="https://unicons.iconscout.com/release/v4.0.0/css/line.css"
  />
  <div class="container">
    <form @submit.prevent="addTodo" class="input-field">
      <input
        v-model="newTodo"
        required
        placeholder="new todo"
        class="input-text"
      />
      <button type="submit" class="todo-button">
        <i class="uil uil-notes note-icon"></i>
      </button>
    </form>

    <ul class="todoLists">
      <li v-for="todo in filteredTodos" :key="todo.id" class="list">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)" class="delete-button">
          <i class="uil uil-trash"></i>
        </button>
      </li>
    </ul>

    <div class="pending-tasks">
      <span>{{ remainingTodos }} tasks remaining</span>
      <button @click="clearAllTodos" class="clear-button">Clear All</button>
    </div>

    <button @click="hideCompleted = !hideCompleted" class="toggle-completed">
      {{ hideCompleted ? "Show all" : "Hide completed" }}
    </button>

    <p>{{ pesan }}</p>
  </div>
</template>

<script setup>
import { ref, computed, onUpdated, watch } from "vue";

let id = 0;
const newTodo = ref("");
const hideCompleted = ref(false);
const todos = ref([{ id: id++, text: "Tugas Prak PBK", done: true }]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});

function addTodo() {
  if (newTodo.value.trim() !== "") {
    todos.value.push({ id: id++, text: newTodo.value, done: false });
    newTodo.value = "";
  }
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t.id !== todo.id);
}

function clearAllTodos() {
  todos.value = [];
}

const remainingTodos = computed(() => {
  return todos.value.filter((todo) => !todo.done).length;
});

onUpdated(() => {
  console.log("Ada suatu perubahan terjadi");
});

const pesan = ref("");
watch(newTodo, (seleksiData) => {
  if (seleksiData.includes("?")) {
    pesan.value = "Tidak boleh mengandung ?";
  } else {
    pesan.value = "";
  }
});
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

html {
  min-height: 100%;
}

body {
  background-image: url("https://source.unsplash.com/random/1920x1080");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  height: 100vh;
  margin: 0;
  padding: 0;
  background-attachment: fixed;
}

.container {
  position: relative;
  max-width: 480px;
  width: 100%;
  border-radius: 8px;
  padding: 25px;
  margin: 85px auto 0;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
}

.input-field {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.input-text {
  flex-grow: 1;
  padding: 10px 15px;
  border: 0px solid #ffffff;
  border-radius: 4px;
  font-size: 16px;
}

.todo-button {
  background: none;
  border: none;
  cursor: pointer;
  margin-left: 10px;
}

.note-icon {
  font-size: 24px;
  color: #707070;
}

.todoLists {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list {
  display: flex;
  align-items: center;
  background: #fff;
  padding: 10px;
  border-radius: 3px;
  margin-bottom: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.list input[type="checkbox"] {
  margin-right: 10px;
}

.list .done {
  text-decoration: line-through;
}

.delete-button {
  background: none;
  border: none;
  cursor: pointer;
  margin-left: auto;
  color: #d9534f;
  font-size: 18px;
}

.pending-tasks {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}

.clear-button {
  background: #d9534f;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.clear-button:hover {
  background: #c9302c;
}

.toggle-completed {
  background: none;
  border: none;
  color: #337ab7;
  cursor: pointer;
  margin-top: 20px;
}

.toggle-completed:hover {
  text-decoration: underline;
}
</style>
