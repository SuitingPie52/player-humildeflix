<template>
    <div class="app">
      <!-- Seu componente de player de vídeo -->
      <VideoPlayer/>
  
      <!-- Conteúdo adicional aqui -->
  
    </div>
  </template>
  
  <script>
  // Importe seu componente de player de vídeo
  import VideoPlayer from './components/VideoBox.vue';
  
  export default {
    name: 'App',
    components: {
      VideoPlayer
    }
  }
  </script>
  
  <style>
  /* Remova as margens padrão do corpo do documento */
  body {
    margin: 0;
    /* Defina a imagem de fundo */
    background-image: url('https://burningthecelluloid.files.wordpress.com/2020/08/2025407.jpg?w=1024');
    /* Defina o tamanho e posicionamento do background */
    background-size: cover;
    background-position: center;
  }
  
</style>
  