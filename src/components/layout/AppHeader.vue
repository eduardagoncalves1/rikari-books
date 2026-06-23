<template>
  <header class="app-header">
    <div class="header-inner">
      <div class="header-left">
        <div class="search-box">
          <input
            type="text"
            placeholder="Buscar manga..."
            :value="busca"
            @input="$emit('buscar', $event.target.value)"
          />
        </div>

        <select class="genre-select" @change="$emit('filtrarGenero', $event.target.value)">
          <option value="">Categoria</option>
          <option v-for="genero in generos" :key="genero" :value="genero">{{ genero }}</option>
        </select>

        <select class="genre-select" @change="$emit('ordenar', $event.target.value)">
          <option value="">Ordenar</option>
          <option value="preco-asc">Menor preço</option>
          <option value="preco-desc">Maior preço</option>
          <option value="recente">Lançamento recente</option>
        </select>
      </div>

      <div class="logo">
        <span class="logo-text">Rikari Books</span>
      </div>

      <div class="header-right">
        <nav class="nav-links">
          <a href="#" @click.prevent="$emit('filtrar', '')" :class="{ active: filtroAtivo === '' }">Todos</a>
          <a href="#" @click.prevent="$emit('filtrar', 'maisVendido')" :class="{ active: filtroAtivo === 'maisVendido' }">Mais Vendidos</a>
          <a href="#" @click.prevent="$emit('filtrar', 'maisBarato')" :class="{ active: filtroAtivo === 'maisBarato' }">Mais Baratos</a>
          <a href="#" @click.prevent="$emit('filtrar', 'favoritos')" :class="{ active: filtroAtivo === 'favoritos' }">
            Favoritos<span class="fav-count" v-if="totalFavoritos > 0">{{ totalFavoritos }}</span>
          </a>
        </nav>

        <button class="cart-btn" @click="$emit('toggleCarrinho')">
          <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="9" cy="21" r="1"/>
            <circle cx="20" cy="21" r="1"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"/>
          </svg>
          <span class="cart-badge" v-if="totalItens > 0">{{ totalItens }}</span>
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  props: {
    totalItens: { type: Number, default: 0 },
    totalFavoritos: { type: Number, default: 0 },
    filtroAtivo: { type: String, default: '' },
    busca: { type: String, default: '' },
    generos: { type: Array, default: () => [] }
  },
  emits: ['toggleCarrinho', 'filtrar', 'filtrarGenero', 'buscar', 'ordenar']
}
</script>

<style scoped>
.app-header {
  background: #3E3F5F;
  padding: 0 2rem;
  position: sticky;
  top: 0;
  z-index: 100;
  box-shadow: 0 4px 20px rgba(62, 63, 95, 0.3);
}

.header-inner {
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  height: 70px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex: 1;
  justify-content: flex-end;
}

.logo {
  flex-shrink: 0;
}

.logo-text {
  font-family: 'Sora', sans-serif;
  font-size: 1.3rem;
  font-weight: 700;
  color: #FDA9E0;
  letter-spacing: 0.5px;
  white-space: nowrap;
}

.nav-links {
  display: flex;
  gap: 0.3rem;
}

.nav-links a {
  color: #FEDBE5;
  text-decoration: none;
  font-size: 0.82rem;
  font-weight: 500;
  padding: 0.4rem 0.7rem;
  border-radius: 20px;
  transition: all 0.2s;
  white-space: nowrap;
}

.nav-links a:hover,
.nav-links a.active {
  background: #FDA9E0;
  color: #3E3F5F;
}

.fav-count {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 16px;
  height: 16px;
  padding: 0 4px;
  margin-left: 5px;
  background: #ff6b9d;
  color: #fff;
  border-radius: 8px;
  font-size: 0.62rem;
  font-weight: 700;
}

.genre-select {
  background: rgba(253, 169, 224, 0.15);
  border: 1px solid rgba(253, 169, 224, 0.4);
  color: #FEDBE5;
  border-radius: 20px;
  padding: 0.4rem 0.8rem;
  font-size: 0.82rem;
  cursor: pointer;
  outline: none;
}

.genre-select option { background: #3E3F5F; color: #fff; }

.search-box {
  position: relative;
  display: flex;
  align-items: center;
}

.search-box input {
  background: rgba(253, 169, 224, 0.12);
  border: 1px solid rgba(253, 169, 224, 0.35);
  color: #fff;
  border-radius: 20px;
  padding: 0.4rem 0.9rem;
  font-size: 0.82rem;
  width: 200px;
  outline: none;
  transition: border-color 0.2s;
}

.search-box input::placeholder { color: rgba(254, 219, 229, 0.6); }
.search-box input:focus { border-color: #FDA9E0; }

.cart-btn {
  background: #FDA9E0;
  border: none;
  border-radius: 50%;
  width: 42px;
  height: 42px;
  cursor: pointer;
  position: relative;
  transition: transform 0.2s, box-shadow 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #3E3F5F;
}

.cart-btn:hover { transform: scale(1.1); box-shadow: 0 4px 12px rgba(253, 169, 224, 0.5); }

.cart-badge {
  position: absolute;
  top: -4px;
  right: -4px;
  background: #ff6b9d;
  color: #fff;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  font-size: 0.65rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (max-width: 900px) {
  .nav-links { display: none; }
  .search-box input { width: 130px; }
  .genre-select { display: none; }
  .logo-text { font-size: 1.1rem; }
}
</style>