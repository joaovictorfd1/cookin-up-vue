<script lang="ts">
import BotaoBuscar from './Buscar.vue'
import { obterCategorias } from '@/http/index';
import type ICategorias from '@/interfaces/ICategorias';
import CardsCategoria from './CardsCategoria.vue';
export default {
    name: 'SelecionarIngredientes',
    data() {
        return {
            categorias: [] as ICategorias[]
        };
    },
    async created() {
        this.categorias = await obterCategorias();
    },
    components: { CardsCategoria, BotaoBuscar },
    emits: ['adicionarIngrediente', 'removerIngrediente', 'buscarReceitas']
}
</script>

<template>
  <section class="selecionar-ingredientes">
    <h1 class="cabecalho titulo-ingredientes">Ingredientes</h1>
    <p class="paragrafo-lg instrucoes">
      Selecione abaixo os ingredientes que você quer usar nesta receita:
    </p>

    <ul class="categorias">
      <li v-for="categoria in categorias" :key="categoria.nome">
        <CardsCategoria
          :categoria="categoria"
          @adicionar-ingrediente="$emit('adicionarIngrediente', $event)"
          @remover-ingrediente="$emit('removerIngrediente', $event)"
        />
      </li>
    </ul>

    <p class="paragrafo dica">
      *Atenção: consideramos que você tem em casa, sal, pimenta e alho.
    </p>

    <BotaoBuscar texto="Buscar receitas!" @click="$emit('buscarReceitas')" />
    <!-- <BotãoBuscar texto="Buscar receitas!" /> -->
  </section>
</template>