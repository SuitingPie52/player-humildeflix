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
      <!-- Barra de tempo -->
      <div class="time-bar" v-if="showButton" @click="seekVideo($event)">
        <div class="progress" :style="{ width: currentTimePercentage }"></div>
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
  
  /* Estilos para a barra de tempo */
  .time-bar {
    width: 100%;
    height: 40px; /* Altura aumentada para tornar mais fácil clicar */
    background-color: #ccc;
    position: absolute;
    bottom: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 10px;
    box-sizing: border-box;
    cursor: pointer;
  }
  
  .progress {
    height: 100%;
    background-color: red;
  }
  
  .time-indicator {
    color: #fff;
    font-size: 12px;
  }
  </style>
  