<script setup>
import MyWrapper from '@/components/MyWrapper.vue'
import { reactive, computed } from 'vue'
import { useRouter } from 'vue-router'
import { usePostsStore } from '@/stores/post'

const router = useRouter()
const postStore = usePostsStore()

const post = reactive({
  title: '',
  body: ''
})

const isFormInvalid = computed(() => {
  return post.title === '' || post.body === ''
})

const handleSubmit = () => {
  postStore.addPost(post)
  router.push({ name: 'home' })
}
</script>

<template>
  <MyWrapper>
   <div class="header">
    <span>Create a New Post</span>
        <div>
            <RouterLink to="/" class="back-link">
            <span class="material-icons">arrow_back</span>
            </RouterLink>
        </div>
    </div>

    <div class="content">
      <form @submit.prevent="handleSubmit">
      <div>
        <label>Post Title</label>
        <input type="text" v-model="post.title" />
      </div>
      <div>
        <label>Post Body</label>
        <textarea rows="7" v-model="post.body"></textarea>
      </div>
      <div>
        <button :disabled="isFormInvalid">Add</button>
      </div>
    </form>
   </div>
  </MyWrapper>
</template>

<style lang="scss" scoped>
.header {
    font-size: 20px;
    background: #1571f0;
    padding: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #fff;
    font-weight: 500;

        .back-link {
        display: flex;
        align-items: center;
        justify-content: center;
        background: #fff;
        color: #1571f0;
        border-radius: 30px;
        padding: 5px;
        text-decoration: none;
        transition: background 0.2s ease-in-out;

        .material-icons {
        font-size: 20px;
        }

        &:hover {
        background: #e0e7ff;
        }
    }
}
.content {
    padding: 1.5rem;
    
    form {
        display: flex;
        flex-direction: column;
        gap: 1.5rem;

        label {
            font-weight: 500;
            margin-bottom: 0.3rem;
            display: block;
        }

        input,
        textarea {
            width: 100%;
            border: 1px solid #333;
            padding: 8px;
            border-radius: 5px;

            &:focus {
                outline: 2px solid #3b82f6;
                border: none;
            }
        }

        button {
            background: #3b82f6;
            color: #fff;
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            font-weight: 600;

            &:hover {
                background: #2563eb;
            }

            &:disabled {
                background: #6293fd;
            }
        }
    }
}
</style>
