<script lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import MostrarReceitas from './MostrarReceitas.vue';
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import Tag from './Tag.vue';
type Pagina = 'SelecionarIngredientes' | 'MostrarReceitas';

export default {
  data() {
    return {
      ingredientes: [] as string[],
      conteudo: 'SelecionarIngredientes' as Pagina,
      loadingTime: '' // Adicionado para armazenar o tempo de carregamento
    };
  },
  components: { SelecionarIngredientes, Tag, MostrarReceitas },
  methods: {
    adicionarIngrediente(ingrediente: string){
      this.ingredientes.push(ingrediente);
    },
    removerIngrediente(ingrediente: string) {
      this.ingredientes = this.ingredientes.filter(iLista => ingrediente !== iLista);
    },
    navegar(pagina: Pagina){
      this.conteudo = pagina;
    }
  },
  mounted() {
    const calculateLoadingTime = () => {
      const now = performance.now();
      const [navigationEntry] = performance.getEntriesByType("navigation");
      const loadTime = (now - navigationEntry.startTime) / 1000;
      this.loadingTime = loadTime.toFixed(2); // Atualiza o estado do componente
    };

    window.addEventListener('load', calculateLoadingTime);

    this.$once('hook:beforeDestroy', () => {
      window.removeEventListener('load', calculateLoadingTime);
    });
  }
}
</script>

<template>
  <main class="conteudo-principal">
    <div class="time">Tempo de carregamento: {{ loadingTime }} segundo ⏱️</div>
    <section>
        <span class="subtitulo-lg sua-lista-texto">
            Sua lista:
        </span>

        <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
            <li v-for="ingrediente in ingredientes" :key="ingrediente">
               <Tag :texto="ingrediente" :class="{ativa: true}"/>
            </li>
        </ul>

        <p v-else class="paragrafo lista-vazia">
            <img src="../assets/images/icones/lista-vazia.svg" alt="ícone de pesquisa">
            Sua lista está vazia, selecione ingredientes para continuar.
        </p>
    </section>
    <KeepAlive include="SelecionarIngredientes">
      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'"
      @adicionar-ingrediente="adicionarIngrediente"
      @remover-ingrediente="removerIngrediente"
      @buscar-receitas="navegar('MostrarReceitas')"
    />
    <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'"
      :ingredientes="ingredientes"
      @editar-receitas="navegar('SelecionarIngredientes')"
    />
    </KeepAlive>
  </main>
</template>

<style scoped>
.conteudo-principal {
  width: 100vw; /* A largura é a da viewport para ocupar toda a largura da tela */
  max-width: 98%;
  margin-left: 1.5vw;
  padding: 6.5rem 0.05rem; /* Remove padding lateral, mantendo vertical */
  border-radius: 3.75rem 3.75rem 0 0;
  background: var(--creme, #FFFAF3);
  color: var(--cinza, #444);
  box-sizing: border-box; /* Inclui padding na largura total */
  display: flex;
  flex-direction: column;
  align-items: center;
  //gap: 5rem;
}
  .time{
    font-weight: bold;
  }

/* Outros estilos permanecem inalterados */

@media only screen and (max-width: 1300px) {
  .conteudo-principal {
    padding: 5rem 0; /* Ajuste de padding para telas menores */
  }
}
.sua-lista-texto {
  color: var(--coral, #F0633C);
  display: block;
  text-align: center;
  margin-bottom: 1.5rem;
}

.ingredientes-sua-lista {
  display: flex;
  justify-content: center;
  gap: 1rem 1.5rem;
  flex-wrap: wrap;
}

.lista-vazia {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.25rem;
  color: var(--coral, #F0633C);
  text-align: center;
}

@media only screen and (max-width: 767px) {
  .conteudo-principal {
    margin-left: 0; 
    padding: 4rem 0; /* Ajuste de padding para dispositivos móveis */
  }
}
</style>
