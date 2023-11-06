<template>
      <div class="content">
        <h1>{{ title }}</h1>
        <p>by matteosantoro.dev</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'BoxInfo',
    props: {
      title: String,
    },
  };
  </script>
  
  <style scoped>
  .content {
    font-size: 1rem;
  }
  </style>  