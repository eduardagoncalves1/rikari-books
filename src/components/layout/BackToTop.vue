
<template>
  <transition name="fade-up">
    <button class="back-to-top" v-if="visivel" @click="voltarAoTopo" aria-label="Voltar ao topo">
      <svg viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2.5">
        <polyline points="18 15 12 9 6 15"/>
      </svg>
    </button>
  </transition>
</template>

<script>
export default {
  name: 'BackToTop',
  data() {
    return { visivel: false }
  },
  mounted() {
    window.addEventListener('scroll', this.checarScroll)
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.checarScroll)
  },
  methods: {
    checarScroll() {
      this.visivel = window.scrollY > 500
    },
    voltarAoTopo() {
      window.scrollTo({ top: 0, behavior: 'smooth' })
    }
  }
}
</script>

<style scoped>
.back-to-top {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  width: 46px;
  height: 46px;
  border-radius: 50%;
  background: #3E3F5F;
  color: #FDA9E0;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 20px rgba(62, 63, 95, 0.35);
  z-index: 150;
  transition: transform 0.2s, background 0.2s;
}

.back-to-top:hover {
  background: #FDA9E0;
  color: #3E3F5F;
  transform: translateY(-3px);
}

.fade-up-enter-active, .fade-up-leave-active { transition: all 0.25s; }
.fade-up-enter-from, .fade-up-leave-to { opacity: 0; transform: translateY(15px); }

@media (max-width: 768px) {
  .back-to-top { bottom: 1.25rem; right: 1.25rem; width: 42px; height: 42px; }
}
</style>