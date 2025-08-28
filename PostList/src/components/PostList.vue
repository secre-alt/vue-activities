<template>
   <div class="PostList">
    <h1>Post List</h1>

     <div v-if="loading">Loading...</div>
    <div v-if="error">{{ error }}</div>

    <table v-if="!loading && !error">
        <thead>
            <tr>
                <th>Id</th>
                <th>Title</th>
                <th>Body</th>
            </tr>
        </thead>
       <tbody>
         <tr v-for="post in posts" :key="post.id">
            <td>{{ post.id }}</td>
            <td>{{ post.title }}</td>
            <td>{{ post.body }}</td>
        </tr>
       </tbody>
    </table>
   </div>
</template>

<script setup lang="ts">
    import { ref, onMounted } from 'vue';
    import axios from 'axios';

const posts = ref([])
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
    try {
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts");
        posts.value = response.data;
    } catch (err) {
        error.value = err.message || "something went wong";
    } finally {
        loading.value = false;
    }
});
</script>

