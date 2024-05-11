<template>
  <div class="video-player">
    <h1 class="title">Humildeflix</h1>
    <div class="video-container">
      <video v-if="videoSrc" controls autoplay class="video">
        <source :src="videoSrc" type="video/mp4">
        Seu navegador não suporta o elemento de vídeo.
      </video>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      videoSrc: ''
    };
  },
  mounted() {
    // Importa o vídeo dinamicamente
    import('@/assets/lahaine.mp4').then(video => {
      this.videoSrc = video.default;
    }).catch(error => {
      console.error('Erro ao carregar o vídeo:', error);
    });
  }
};
</script>

<style scoped>
.video-player {
  font-family: Baskerville, serif;
  background-color: #000;
  color: #ff0000;
  padding: 20px;
  border-radius: 8px;
  max-width: 800px;
  margin: 0 auto;
  margin-top: 200px;
  border: 2px solid transparent; /* Adicionando uma borda de 2px */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Adicionando sombra suave */
  transition: border-color 0.3s ease, box-shadow 0.3s ease; /* Transição suave para borda e sombra */
}

.video-player:hover {
  border-color: #ff0000; /* Alterando a cor da borda ao passar o mouse */
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2); /* Alterando a sombra ao passar o mouse */
}

.title {
  font-size: 24px;
  margin-bottom: 10px;
  text-align: center; /* Alinhar o título ao centro */
}

.video-container {
  margin-top: 20px;
  width: 100%;
  height: 0;
  padding-bottom: 56.25%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.video {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: contain; /* Ajustar o tamanho do vídeo */
  top: 0;
  left: 0;
}
</style>
