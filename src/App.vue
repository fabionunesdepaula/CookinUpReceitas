<script lang="ts">
import Banner from './components/Banner.vue';
import ConteudoPrincipal from './components/ConteudoPrincipal.vue';
import Rodape from './components/Rodape.vue';

export default {
  components: { Banner, ConteudoPrincipal, Rodape }
}
</script>

<template>
  <div class="main-container">
    <Banner />
    <ConteudoPrincipal />
    <Rodape />
  </div>
</template>

<style scoped>
.main-container {
  display: flex;
  flex-direction: column;
  width: 98vw; /* A largura é a da viewport para evitar deslocamento */
  margin-left: -10%; /* Remove qualquer margem padrão */
  padding: 0; /* Remove qualquer padding padrão */
  /*overflow-x: hidden; /* Previne rolagem horizontal indesejada */
}
</style>
