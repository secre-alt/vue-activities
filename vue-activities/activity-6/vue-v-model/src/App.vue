<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
      <nav>
        <RouterLink to="/" class="nav-link">
            <span class="material-icons">home</span>
            Home</RouterLink>
        <RouterLink to="/post-create" class="nav-link">
           <span class="material-icons">add_circle</span>
            New Post</RouterLink>
      </nav>
  </header>
  <RouterView />
</template>

<style lang="scss" scoped>
  nav {
    padding: 1rem 2rem;
    background: #1571f0;
    border-bottom: 1px solid #60a5fa;

    .nav-link {
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
      color: #fff;
      padding: 5px 15px;
      border-radius: 5px;
      transition: all 0.2s ease-in-out;
      margin-right: 1rem;
      position: relative;
      text-decoration: none;

      .material-icons {
        font-size: 20px;
      }
      
      &::after {
      content: '';
      position: absolute;
      left: 0;
      bottom: 0;
      width: 0;
      height: 2px;
      background: #6eafff;
      transition: width 0.3s ease-in-out;
    }

    &:hover::after {
      width: 100%;
    }
    }
  }
</style>