<template>
    <div>
      <img :src="imageSrc" :alt="imageAlt">
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        imageSrc: 'https://via.placeholder.com/150',
        imageAlt: 'Imagem de Exemplo'
      };
    }
  };
  </script>
  