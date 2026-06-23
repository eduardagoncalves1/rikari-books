<template>
  <section class="product-list-section" :class="{ destaque: destaque }">
    <div class="section-header" v-if="titulo">
      <h2 class="section-title">{{ titulo }}</h2>
      <span class="section-count">{{ produtos.length }} {{ produtos.length === 1 ? 'título' : 'títulos' }}</span>
    </div>

    <div class="product-grid" v-if="produtos.length > 0">
      <ProductCard
        v-for="produto in produtos"
        :key="produto.id"
        :produto="produto"
        :favoritos="favoritos"
        @adicionarAoCarrinho="$emit('adicionarAoCarrinho', $event)"
        @toggleFavorito="$emit('toggleFavorito', $event)"
        @verDetalhes="$emit('verDetalhes', $event)"
      />
    </div>

    <div class="empty-state" v-else>
      <p>Nenhum título encontrado.</p>
      <small>Tente outro termo de busca ou filtro.</small>
    </div>
  </section>
</template>

<script>
import ProductCard from './ProductCard.vue'

export default {
  name: 'ProductList',
  components: { ProductCard },
  props: {
    produtos: { type: Array, default: () => [] },
    favoritos: { type: Array, default: () => [] },
    titulo: { type: String, default: '' },
    destaque: { type: Boolean, default: false }
  },
  emits: ['adicionarAoCarrinho', 'toggleFavorito', 'verDetalhes']
}
</script>

<style scoped>
.product-list-section { padding: 0 0 3rem; }

.product-list-section.destaque {
  background: linear-gradient(180deg, rgba(253,169,224,0.08), transparent);
  border-radius: 20px;
  padding: 1.5rem 1.5rem 2.5rem;
  margin: 0 -1.5rem 1rem;
}

.section-header {
  display: flex;
  align-items: baseline;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.section-title {
  font-size: 1.4rem;
  font-weight: 700;
  color: #3E3F5F;
  position: relative;
}

.section-title::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -4px;
  width: 100%;
  height: 3px;
  background: linear-gradient(90deg, #FDA9E0, #FEDBE5);
  border-radius: 2px;
}

.section-count {
  font-size: 0.82rem;
  color: #aaa;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(185px, 1fr));
  gap: 1.25rem;
}

.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  color: #bbb;
}

.empty-state p { font-size: 1.1rem; color: #999; margin: 0.25rem 0; }
.empty-state small { font-size: 0.85rem; }
</style>