<script lang="ts">
import Rodape from './Rodape.vue';
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import Tag from './Tag.vue';
import MostrarReceitasVue from './MostrarReceitas.vue';

type Pagina = 'SelecionarIngredientes' | 'MostrarReceitas'

export default {
  data() {
    return {
      ingredientes: [] as string[],
      conteudo: 'SelecionarIngredientes' as Pagina
    };
  },
  components: { SelecionarIngredientes, Tag, Rodape, MostrarReceitasVue },
  methods: {
    adicionarIngrediente(ingrediente: string) {
      this.ingredientes.push(ingrediente)
    },
    removerIngrediente(ingrediente: string) {
      this.ingredientes = this.ingredientes.filter(item => ingrediente !== item)
    },
    navegar(pagina: Pagina) {
      this.conteudo = pagina
    }
  }
}
</script>

<template>
  <main class="conteudo-principal">
    <section>
      <span class="subtitulo-lg sua-lista-texto">
        Sua lista:
      </span>
      <ul v-if="ingredientes.length > 0" class="ingredientes-sua-lista">
        <li v-for="ingrediente in ingredientes" :key="ingrediente">
          <Tag :texto="ingrediente" ativa />
        </li>
      </ul>

      <p v-else class="paragrafo lista-vazia">
        <img src="../assets/imagens/icones/lista-vazia.svg" alt="">
        Sua lista está vazia, selecione ingredientes para iniciar.
      </p>
    </section>

    <KeepAlive include="SelecionarIngredientes">
      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'"
        @adicionar-ingrediente="adicionarIngrediente($event)" @remover-ingrediente="removerIngrediente($event)"
        @buscar-receitas="navegar('MostrarReceitas')" />

      <MostrarReceitasVue v-else-if="conteudo === 'MostrarReceitas'" @editar-lista="navegar('SelecionarIngredientes')"
        :ingredientes="ingredientes" />
    </KeepAlive>
    <Rodape />
  </main>
</template>