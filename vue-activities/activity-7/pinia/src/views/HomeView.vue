<script setup>
import PostItem from '@/components/PostItem.vue';
import MyWrapper from '@/components/MyWrapper.vue';
import { usePostsStore } from '@/stores/post';

const PostStore = usePostsStore()

</script>

<template>
 <div v-for="post in PostStore.sorted" :key="post.id">
  <MyWrapper>
    <PostItem :post="post" />
  </MyWrapper> 
 </div>
</template>

