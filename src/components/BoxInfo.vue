<template>
      <div class="content">
        <h1>{{ title }}</h1>
        <p>Scegli il colore desiderato utilizzando il selettore e copia il valore di cui hai bisogno.</p>
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