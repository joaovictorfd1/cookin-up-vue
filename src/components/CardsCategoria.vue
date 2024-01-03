<script lang="ts">
import type ICategorias from '@/interfaces/ICategorias';
import type { PropType } from 'vue';
import Tag from './Tag.vue';
import IngredienteSelecionavel from './IngredienteSelecionavel.vue';

export default {
  props: {
    categoria: { type: Object as PropType<ICategorias>, required: true }
  },
  components: { Tag, IngredienteSelecionavel },
  emits: ['adicionarIngrediente', 'removerIngrediente'],
}
</script>

<template>
  <article class="categoria">
    <header class="categoria__cabecalho">
      <img :src="`/imagens/icones/categorias_ingredientes/${categoria.imagem}`" alt="" class="categoria__imagem">
      <h2 class="paragrafo-lg categoria__nome">
        {{ categoria.nome }}
      </h2>
    </header>

    <ul class="categoria__ingredientes">
      <li v-for="ingrediente in categoria.ingredientes" :key="ingrediente">
        <IngredienteSelecionavel 
          :ingrediente="ingrediente"
          @adicionar-ingrediente="$emit('adicionarIngrediente', $event)"
          @remover-ingrediente="$emit('removerIngrediente', $event)"
        />
      </li>
    </ul>
  </article>
</template>