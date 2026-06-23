<template>
  <div id="app">
    <AppHeader
      :totalItens="totalItensCarrinho"
      :totalFavoritos="favoritos.length"
      :filtroAtivo="filtroAtivo"
      :busca="busca"
      :generos="generos"
      @toggleCarrinho="carrinhoVisivel = !carrinhoVisivel"
      @filtrar="filtrar"
      @filtrarGenero="filtrarGenero"
      @buscar="buscar"
      @ordenar="ordenarPor = $event"
    />

    <main class="main-content">
      <HomeView
        :produtos="produtos"
        :favoritos="favoritos"
        :filtroAtivo="filtroAtivo"
        :filtroGenero="filtroGeneroAtivo"
        :busca="busca"
        :ordenarPor="ordenarPor"
        @adicionarAoCarrinho="adicionarAoCarrinho"
        @toggleFavorito="toggleFavorito"
      />
    </main>

    <CartPanel
      :carrinho="carrinho"
      :visivel="carrinhoVisivel"
      @fechar="carrinhoVisivel = false"
      @incrementar="incrementarQuantidade"
      @decrementar="decrementarQuantidade"
      @remover="removerDoCarrinho"
      @finalizar="finalizarCompra"
      @limpar="limparCarrinho"
    />

    <transition name="toast">
      <div class="toast" v-if="toast.visivel">{{ toast.mensagem }}</div>
    </transition>

    <BackToTop />
  </div>
</template>

<script>
import AppHeader from './components/layout/AppHeader.vue'
import HomeView from './components/layout/HomeView.vue'
import CartPanel from './components/cart/CartPanel.vue'
import BackToTop from './components/layout/BackToTop.vue'
import produtos from './data/products.js'
import {
  adicionarAoCarrinho as addCart,
  removerDoCarrinho as removeCart,
  incrementarQuantidade as incrementCart,
  decrementarQuantidade as decrementCart,
  limparCarrinho as clearCart,
  totalItens,
  recalcularTotal
} from './utils/cartUtils.js'
import { calcularFrete } from './utils/currencyUtils.js'

export default {
  name: 'App',
  components: { AppHeader, HomeView, CartPanel, BackToTop },
  data() {
    return {
      produtos,
      carrinho: {
        items: [],
        frete: 0,
        desconto: 0,
        total: 0
      },
      favoritos: [],
      carrinhoVisivel: false,
      filtroAtivo: '',
      filtroGeneroAtivo: '',
      busca: '',
      ordenarPor: '',
      toast: { visivel: false, mensagem: '' }
    }
  },
  computed: {
    totalItensCarrinho() {
      return totalItens(this.carrinho)
    },
    generos() {
      return [...new Set(this.produtos.map(p => p.genero))].sort()
    }
  },
  methods: {
    adicionarAoCarrinho(produto) {
      addCart(this.carrinho, produto)
      this.atualizarFrete()
      this.mostrarToast(`"${produto.titulo}" adicionado ao carrinho`)
    },
    removerDoCarrinho(itemId) {
      removeCart(this.carrinho, itemId)
      this.atualizarFrete()
    },
    incrementarQuantidade(itemId) {
      incrementCart(this.carrinho, itemId)
      this.atualizarFrete()
    },
    decrementarQuantidade(itemId) {
      decrementCart(this.carrinho, itemId)
      this.atualizarFrete()
    },
    limparCarrinho() {
      clearCart(this.carrinho)
    },
    atualizarFrete() {
      const subtotal = this.carrinho.items.reduce((a, i) => a + i.valorTotal, 0)
      this.carrinho.frete = calcularFrete(subtotal)
      recalcularTotal(this.carrinho)
    },
    toggleFavorito(produtoId) {
      const idx = this.favoritos.indexOf(produtoId)
      if (idx === -1) {
        this.favoritos.push(produtoId)
        this.mostrarToast('Adicionado aos favoritos')
      } else {
        this.favoritos.splice(idx, 1)
        this.mostrarToast('Removido dos favoritos')
      }
    },
    filtrar(tipo) {
      this.filtroAtivo = tipo
      this.busca = ''
      this.filtroGeneroAtivo = ''
    },
    filtrarGenero(genero) {
      this.filtroGeneroAtivo = genero
      this.filtroAtivo = ''
      this.busca = ''
    },
    buscar(termo) {
      this.busca = termo
      this.filtroAtivo = ''
      this.filtroGeneroAtivo = ''
    },
    finalizarCompra() {
      this.mostrarToast('Compra finalizada com sucesso')
      clearCart(this.carrinho)
      this.carrinhoVisivel = false
    },
    mostrarToast(mensagem) {
      this.toast = { visivel: true, mensagem }
      setTimeout(() => { this.toast.visivel = false }, 2500)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700;800&family=Nunito:wght@400;500;600;700;800&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'Nunito', sans-serif;
  background: #FFF5F9;
  color: #3E3F5F;
  min-height: 100vh;
}

#app { min-height: 100vh; }

.main-content {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem 4rem;
}

.toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  background: #3E3F5F;
  color: #FDA9E0;
  padding: 0.75rem 1.5rem;
  border-radius: 30px;
  font-size: 0.9rem;
  font-weight: 600;
  z-index: 500;
  box-shadow: 0 8px 25px rgba(62, 63, 95, 0.35);
  white-space: nowrap;
}

.toast-enter-active, .toast-leave-active { transition: all 0.3s; }
.toast-enter-from, .toast-leave-to { opacity: 0; transform: translateX(-50%) translateY(20px); }

::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: #FFF5F9; }
::-webkit-scrollbar-thumb { background: #FDA9E0; border-radius: 3px; }
</style>
