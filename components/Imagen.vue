<template>
    <div class="imagen">
      <img :src="imagen" alt="no carga la imagen" />
      <h2>{{ nombre }}</h2>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      imagen: {
        type: String,
        required: true,
      },
      nombre: {
        type: String,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .imagen {
    display: flex;
    align-items: center;
    flex-direction: column;
    
  }
  img {
    width: 200px;
    height: 250px;
   
  }
  </style>
  