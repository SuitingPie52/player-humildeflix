<template>
  <div class="background-wallpaper"></div>
</template>

<script>
export default {
  name: 'BackgroundWallpaper',
  mounted() {
    // Altere o URL da imagem de acordo com o seu wallpaper desejado
    document.querySelector('.background-wallpaper').style.backgroundImage = "url('https://wallpapercave.com/wp/BAivmTK.jpg')";
    // Adicione estilos CSS aqui se necessário
  },
};
</script>

<style scoped>
.background-wallpaper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  /* Adicione outros estilos de background aqui, como tamanho, repetição, etc. */
}
</style>
