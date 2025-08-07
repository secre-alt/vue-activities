<script lang="ts" setup>
import { ref } from 'vue';

const message = ref('To Do List');
const newTask = ref('');
const tasks = ref<{ text: string; completed: boolean }[]>([]); 

function formSubmitted() {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ text: newTask.value.trim(), completed: false });
    newTask.value = ''; 
  }
}


function deleteTask(index: number) {
  tasks.value.splice(index, 1);
}


function editTask(index: number) {
  const editedTask = prompt('Edit your task:', tasks.value[index].text);
  if (editedTask !== null && editedTask.trim() !== '') {
    tasks.value[index].text = editedTask.trim();
  }
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>

    <form @submit.prevent="formSubmitted">
      <label>
        <input v-model="newTask" name="newTask" placeholder="Enter task" />
      </label>
      <div class="button-container">
        <button type="submit">Add</button>
      </div>
    </form>

    <!-- Task List -->
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }">
        <!-- Circular checkbox -->
        <input type="checkbox" v-model="task.completed" class="task-checkbox" />
        <span>{{ task.text }}</span>
        <button @click="editTask(index)" class="edit-btn">
          <i class="fas fa-edit"></i>
        </button>
        <button @click="deleteTask(index)" class="delete-btn">
          <i class="fas fa-trash-alt"></i>
        </button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
main {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1.5rem;
  background-color: #ffffff; 
  border-radius: 10px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
  color: #222;
}

h1 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
  color: #222;
  text-align: center;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #333;
  justify-content: center;
  align-items: center;
}

input {
  padding: 0.5rem;
  width: 100%;
  margin-top: 0.25rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  background-color: #f5f5f5;
  color: #000;
}

.button-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 0.5rem;
}

button {
  padding: 0.5rem 1.5rem;
  background-color: #3b82f6;
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

button:hover {
  background-color: #2563eb;
}

.task-list {
  list-style-type: disc;
  margin-top: 1.5rem;
  padding-left: 1.5rem;
}

.task-list li {
  margin-bottom: 0.5rem;
  color: #222;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-list li.completed span {
  text-decoration: line-through;
  color: #888;
}

.edit-btn, .delete-btn {
  margin-left: 0.5rem;
  padding: 0.5rem;
  background-color: transparent;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease;
  font-size: 1.1rem;
}

.edit-btn {
  color: #fca311;
}

.edit-btn:hover {
  background-color: #f98404;
}

.delete-btn {
  color: #d14f4f;
}

.delete-btn:hover {
  background-color: #c63f3f;
}

/* Icon styling */
.edit-btn i, .delete-btn i {
  font-size: 1rem;
}



/* Circular Checkbox Styling */
input[type="checkbox"].task-checkbox {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 2px solid #3b82f6;
  background-color: white;
  cursor: pointer;
  position: relative;
  transition: background-color 0.2s ease, border-color 0.2s ease;
}

input[type="checkbox"].task-checkbox:checked {
  background-color: #3b82f6;
  border-color: #2563eb;
}

input[type="checkbox"].task-checkbox:checked::before {
  content: '';
  position: absolute;
  top: 4px;
  left: 4px;
  width: 8px;
  height: 8px;
  background-color: white;
  border-radius: 50%;
}

input[type="checkbox"].task-checkbox:hover {
  background-color: #f5f5f5;
}
</style>
