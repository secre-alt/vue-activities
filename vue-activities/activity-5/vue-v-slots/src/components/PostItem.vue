<script setup>

defineProps({
post: {
    type: Object,
    required: true
}
})
</script>

<template>
    <div>
     <div class="header">
        <span>POST ID NO. {{ post.id }}</span>
        <div>
            <button class="del material-icons">delete</button>
            <button class="save material-icons">bookmark_border</button>
        </div>
        </div>
        <div class="content">
        <h1>{{ post.title }}</h1>
        <p>{{ post.body}}</p>
        </div>
    </div>
</template>


<style lang="scss" setup>
  .header {
    font-size: 20px;
    font-style: bold;
    background: #0284c7;
    padding: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #fff;
    font-weight: 300;
    button {
      border-radius: 30px;
    }

    div {
      display: flex;
      gap: 0.5rem;
      align-items: center;

       .del {
          font-size: 20px;
          padding: 3px;
          background: #fff;
          color: #ef4444;   
        }
        .save {
          font-size: 20px;
          padding: 3px;
          background: #fff;
          color: #14a841;
      }
    }
}
   .content {
    padding: 1rem;
    
     h1 {
      font-size: 1.5rem;
      font-weight: 700;
      margin-bottom: 0.5rem;
      color: #1e293b;
      text-transform: capitalize;
    }

    p {
      font-size: 1rem;
      line-height: 1.5;
      color: #475569;
      white-space: pre-line;
    }
}
</style>