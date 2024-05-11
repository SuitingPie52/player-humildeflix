<template>
    <div class="video-container">
      <video v-if="videoSrc" ref="videoPlayer" autoplay class="video" @click="togglePlayPause">
        <source :src="videoSrc" type="video/mp4">
        Seu navegador não suporta o elemento de vídeo.
      </video>
      <!-- Botão de pause/play dentro do vídeo -->
      <button class="play-pause-button-video" :class="{ playing: isPlaying }" @click="togglePlayPause">
        {{ isPlaying ? '❚❚' : '▶' }}
      </button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      videoSrc: {
        type: String,
        required: true
      }
    },
    data() {
      return {
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
    }
  };
  </script>
  
  <style scoped>
  /* Estilos para o player de vídeo */
  .video-container {
    position: relative;
    width: 100%;
  }
  
  .video {
    width: 100%;
    height: auto;
  }
  
  .play-pause-button-video {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: rgba(255, 0, 0, 0.5);
    color: #fff;
    border: none;
    padding: 10px;
    cursor: pointer;
    border-radius: 50%;
    font-size: 24px;
    transition: background-color 0.3s ease;
    z-index: 1; /* Certifica-se de que o botão está na frente do vídeo */
    opacity: 0; /* Oculta o botão por padrão */
  }
  
  .play-pause-button-video.playing {
    opacity: 1; /* Exibe o botão quando o vídeo estiver em reprodução */
  }
  
  .play-pause-button-video:hover {
    background-color: rgba(255, 0, 0, 0.8);
  }
  </style>
  