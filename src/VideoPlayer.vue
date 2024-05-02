User
<template>
  <div class="video-player">
    <h1 class="title">Humildeflix</h1>
    <p class="welcome-message">Bem-vindo ao player Humildeflix. Basta colar o link do YouTube que você quiser e o player será acionado.</p>
    <div class="copy-container">
      <p class="suggestion">Sugestão de link:</p>
      <div class="copy-button-container">
        <button @click="copySuggestionLink">Copiar</button>
        <p v-if="showCopyMessage" class="confirmation-message">Link copiado com sucesso!</p>
      </div>
    </div>
    <div class="link-input">
      <input v-model="videoLink" placeholder="Cole o link do vídeo aqui" @input="submitLink">
    </div>
    <p v-if="!videoSrc && !showError && !videoLink" class="no-link-message">Nenhum link válido utilizado.</p>
    <div class="video-container">
      <iframe v-if="videoSrc" :src="videoSrc" frameborder="0" allowfullscreen></iframe>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    autoplay: Boolean,
    volume: Number
  },
  data() {
    return {
      videoLink: '',
      videoSrc: '',
      showError: false,
      linkCopied: false,
      showCopyMessage: false
    };
  },
  methods: {
    submitLink() {
      if (this.videoLink && this.isYouTubeLink(this.videoLink)) {
        this.videoSrc = this.getYoutubeEmbedUrl(this.videoLink);
        this.showError = false;
      } else {
        this.showError = true;
      }
    },
    isYouTubeLink(url) {
      return url.includes('youtube.com') || url.includes('youtu.be');
    },
    getYoutubeEmbedUrl(url) {
      const videoId = this.getYouTubeVideoId(url);
      if (videoId) {
        return `https://www.youtube.com/embed/${videoId}`;
      } else {
        console.error('URL do YouTube inválida');
        return null;
      }
    },
    getYouTubeVideoId(url) {
      const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
      const match = url.match(regExp);
      if (match && match[2].length == 11) {
        return match[2];
      } else {
        console.error('URL do YouTube inválida');
        return null;
      }
    },
    copySuggestionLink() {
      const suggestionLink = "https://www.youtube.com/watch?v=aZFd-y_EUfM";
      navigator.clipboard.writeText(suggestionLink).then(() => {
        this.showCopyMessage = true;
        setTimeout(() => {
          this.showCopyMessage = false;
        }, 3000);
      }).catch(() => {
        console.error('Erro ao copiar o link');
      });
    }
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
  margin-top: 150px;
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
  margin-left: 310px;
}

.welcome-message {
  font-size: 16px;
  margin-bottom: 20px;
  color: #fff;
}

.copy-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.suggestion {
  margin-right: 10px;
}

.copy-button-container {
  display: flex;
  align-items: center;
}

.confirmation-message {
  margin-left: 10px;
  color: green;
}

.link-input {
  margin-bottom: 20px;
}

.no-link-message {
  margin-top: 20px;
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

.video-container iframe {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}
</style>



