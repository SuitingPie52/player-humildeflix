<template>
  <div class="video-box">
    <h1 class="title">Humildeflix</h1>
    <VideoPlayer :videoSrc="videoSrc" />
  </div>
</template>

<script>
import VideoPlayer from '@/components/VideoPlayer.vue';

export default {
  components: {
    VideoPlayer
  },
  data() {
    return {
      videoSrc: '',
      isPlaying: false
    };
  },
  methods: {
    togglePlayPause() {
      const video = this.$refs.videoPlayer;
      if (video.paused) {
        video.play();
        this.isPlaying = true;
      } else {
        video.pause();
        this.isPlaying = false;
      }
    }
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
.video-box {
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

.video-box:hover {
  border-color: #ff0000; /* Alterando a cor da borda ao passar o mouse */
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2); /* Alterando a sombra ao passar o mouse */
}

.title {
  font-size: 24px;
  margin-bottom: 10px;
  text-align: center; /* Alinhar o título ao centro */
}

</style>
