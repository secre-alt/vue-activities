<script lang="ts" setup>
import { ref } from 'vue'


const message = ref('Task');
const newTask = ref("");

function formSubmitted() {
  console.log(newTask.value);
}
</script>

<template>
   <main>
     <h1>{{ message }}</h1>
     <form>
        <label>
          New Task
          <input v-model="newTask" name="newTask"/>
        </label>
        <div class=" button-container">
          <button>Add</button>
        </div>
        <!-- <h3>{{ newTask }}</h3> -->
      </form>

    </main>
  </template>


  <style scoped>
  main {
    max-width: 800px;
    margin: 1rem auto
  }

  .button-container {
    display: flex;
    justify-content: end;
  }
  </style>