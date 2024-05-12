<template>
    <div class="video-container" :style="{ height: containerHeight }" @mouseover="showControls" @mouseleave="hideControls">
      <video v-if="videoSrc" ref="videoPlayer" autoplay class="video" @click="togglePlayPause" @timeupdate="updateTime">
        <source :src="videoSrc" type="video/mp4">
        Seu navegador não suporta o elemento de vídeo.
      </video>
      <!-- Botão de pause/play dentro do vídeo -->
      <transition name="fade">
        <button v-show="showButton" class="play-pause-button-video" :class="{ playing: isPlaying }" @click="togglePlayPause">
          <span class="play-pause-icon">{{ isPlaying ? '▶' : '❚❚' }}</span>
        </button>
      </transition>
      <!-- Barra de tempo total -->
      <div class="total-time-bar"></div>
      <!-- Barra de tempo assistido -->
      <div class="time-bar" v-if="showButton" @click="seekVideo($event)">
        <div class="progress" :style="{ width: currentTimePercentage }"></div>
        <!-- Indicador de tempo -->
        <div class="time-indicator">{{ currentTime }}</div>
      </div>
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
        isPlaying: false,
        containerHeight: "auto",
        showButton: false,
        currentTimePercentage: "0%",
        currentTime: "0:00"
      };
    },
    mounted() {
      this.updateContainerHeight();
      window.addEventListener("resize", this.updateContainerHeight);
    },
    beforeUnmount() {
      window.removeEventListener("resize", this.updateContainerHeight);
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
      },
      updateContainerHeight() {
        const video = this.$refs.videoPlayer;
        if (video) {
          this.containerHeight = `${video.clientHeight}px`;
        }
      },
      showControls() {
        this.showButton = true;
      },
      hideControls() {
        this.showButton = false;
      },
      updateTime() {
        const video = this.$refs.videoPlayer;
        const percentage = (video.currentTime / video.duration) * 100;
        this.currentTimePercentage = `${percentage}%`;
        
        const minutes = Math.floor(video.currentTime / 60);
        const seconds = Math.floor(video.currentTime % 60);
        this.currentTime = `${minutes}:${seconds.toString().padStart(2, "0")}`;
      },
      seekVideo(event) {
        const video = this.$refs.videoPlayer;
        const boundingRect = event.currentTarget.getBoundingClientRect();
        const percentage = (event.clientX - boundingRect.left) / boundingRect.width;
        video.currentTime = video.duration * percentage;
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
    padding: 0;
    cursor: pointer;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .play-pause-icon {
    font-size: 24px;
  }
  
  .play-pause-button-video.playing {
    opacity: 1;
  }
  
  .play-pause-button-video:hover {
    background-color: rgba(255, 0, 0, 0.8);
  }
  
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 1s;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
  
  .time-bar {
    width: calc(100% - 10px); /* Reduz o comprimento da barra de tempo */
    height: 5px; /* Reduz a altura da barra de tempo */
    background-color: rgba(211, 211, 211, 0.5); /* Define o fundo transparente */
    position: absolute;
    bottom: -30px; /* Ajusta a posição vertical */
    left: 10px; /* Ajusta a posição horizontal */
    cursor: pointer;
  }
  
  .progress {
    height: 100%;
    background-color: red;
  }
  
  .time-indicator {
    position: absolute;
    bottom: -20px; /* Ajusta a posição vertical */
    right: 10px; /* Posiciona no canto inferior direito */
    color: #fff;
    font-size: 12px;
  }
  </style>
  