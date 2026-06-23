<template>
  <div class="cart-summary">
    <div class="summary-row">
      <span>Subtotal</span>
      <span>{{ formatarPreco(subtotal) }}</span>
    </div>
    <div class="summary-row frete-row">
      <span>Frete</span>
      <span :class="{ gratis: carrinho.frete === 0 }">
        {{ carrinho.frete === 0 ? 'Grátis' : formatarPreco(carrinho.frete) }}
      </span>
    </div>
    <div class="summary-row desconto-row" v-if="carrinho.desconto > 0">
      <span>Desconto</span>
      <span class="desconto">− {{ formatarPreco(carrinho.desconto) }}</span>
    </div>
    <div class="frete-info" v-if="carrinho.frete > 0">
      <span>Frete grátis em compras acima de R$ 150,00</span>
    </div>
    <div class="summary-total">
      <span>Total</span>
      <span>{{ formatarPreco(carrinho.total) }}</span>
    </div>
    <button class="checkout-btn" @click="$emit('finalizar')" :disabled="carrinho.items.length === 0">
      Finalizar compra
    </button>
    <button class="clear-btn" @click="$emit('limpar')" v-if="carrinho.items.length > 0">
      Limpar carrinho
    </button>
  </div>
</template>

<script>
import { formatarPreco } from '../../utils/currencyUtils.js'

export default {
  name: 'CartSummary',
  props: { carrinho: { type: Object, required: true } },
  emits: ['finalizar', 'limpar'],
  computed: {
    subtotal() {
      return this.carrinho.items.reduce((acc, i) => acc + i.valorTotal, 0)
    }
  },
  methods: { formatarPreco }
}
</script>

<style scoped>
.cart-summary {
  padding: 1rem 0 0;
  border-top: 2px solid #FEDBE5;
  margin-top: 0.5rem;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.85rem;
  color: #666;
  padding: 0.3rem 0;
}

.gratis { color: #5cb85c; font-weight: 700; }
.desconto { color: #e74c3c; font-weight: 600; }

.frete-info {
  font-size: 0.72rem;
  color: #aaa;
  text-align: center;
  margin: 0.2rem 0;
}

.summary-total {
  display: flex;
  justify-content: space-between;
  font-size: 1.1rem;
  font-weight: 700;
  color: #3E3F5F;
  padding: 0.6rem 0;
  border-top: 1px dashed #FEDBE5;
  margin-top: 0.4rem;
}

.checkout-btn {
  width: 100%;
  background: linear-gradient(135deg, #FDA9E0, #f87ec9);
  color: #3E3F5F;
  border: none;
  border-radius: 14px;
  padding: 0.85rem;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  margin-top: 0.75rem;
  transition: transform 0.2s, box-shadow 0.2s;
}

.checkout-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(253, 169, 224, 0.5);
}

.checkout-btn:disabled { opacity: 0.5; cursor: not-allowed; }

.clear-btn {
  width: 100%;
  background: none;
  border: none;
  color: #ccc;
  font-size: 0.78rem;
  cursor: pointer;
  margin-top: 0.5rem;
  padding: 0.3rem;
  transition: color 0.2s;
}

.clear-btn:hover { color: #e74c3c; }
</style>