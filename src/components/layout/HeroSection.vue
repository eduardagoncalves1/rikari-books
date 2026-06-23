<template>
  <section class="hero">
    <div class="sakura-layer" aria-hidden="true">
      <span class="petal" v-for="n in 14" :key="n" :style="petalStyle(n)"></span>
    </div>

    <div class="hero-content">
      <p class="hero-eyebrow">A livraria de mangas que voce procurava</p>
      <h1 class="hero-title">Rikari Books</h1>
      <p class="hero-slogan">Onde cada pagina leva voce para um novo universo</p>

      <div class="hero-stats">
        <div class="stat"><strong>50+</strong><span>Titulos</span></div>
        <div class="stat-div"></div>
        <div class="stat"><strong>12+</strong><span>Generos</span></div>
        <div class="stat-div"></div>
        <div class="stat"><strong>Gratis</strong><span>Frete acima de R$150</span></div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HeroSection',
  methods: {
    petalStyle(n) {
      const left = (n * 53) % 100
      const delay = (n % 9) * 1.3
      const duration = 9 + (n % 5) * 2
      const size = 10 + (n % 4) * 4
      return {
        left: left + '%',
        animationDelay: delay + 's',
        animationDuration: duration + 's',
        width: size + 'px',
        height: size + 'px'
      }
    }
  }
}
</script>

<style scoped>
.hero {
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  min-height: 420px;
  padding: 4rem 2rem;
  border-radius: 28px;
  margin-top: 1.5rem;
  background-image:
    linear-gradient(155deg, rgba(255,240,246,0.93) 0%, rgba(255,227,238,0.88) 45%, rgba(253,234,243,0.93) 100%),
    url('https://images.unsplash.com/photo-1585003387496-7ef0cc60267d?auto=format&fit=crop&w=1600&q=80');
  background-size: cover;
  background-position: center;
}

.sakura-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.petal {
  position: absolute;
  top: -20px;
  display: block;
  background: radial-gradient(circle at 30% 30%, #ffffff 0%, #FDA9E0 70%);
  border-radius: 0 60% 0 60%;
  opacity: 0.75;
  animation-name: fall;
  animation-timing-function: ease-in;
  animation-iteration-count: infinite;
}

@keyframes fall {
  0% { transform: translateY(-30px) rotate(0deg); opacity: 0; }
  10% { opacity: 0.8; }
  100% { transform: translateY(420px) rotate(280deg); opacity: 0; }
}

.hero-content {
  position: relative;
  z-index: 1;
  max-width: 640px;
}

.hero-eyebrow {
  font-family: 'Sora', sans-serif;
  font-size: 0.8rem;
  font-weight: 600;
  color: #d6699f;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  margin: 0 0 0.9rem;
}

.hero-title {
  font-family: 'Sora', sans-serif;
  font-weight: 800;
  font-size: 3.4rem;
  letter-spacing: -1px;
  color: #3E3F5F;
  margin: 0 0 1rem;
  line-height: 1.05;
}

.hero-slogan {
  font-family: 'Sora', sans-serif;
  font-size: 1.05rem;
  font-weight: 600;
  font-style: italic;
  color: #3E3F5F;
  line-height: 1.6;
  margin: 0 auto 1.75rem;
  max-width: 460px;
}

.hero-stats {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.25rem;
}

.stat { display: flex; flex-direction: column; gap: 2px; }
.stat strong { font-family: 'Sora', sans-serif; font-size: 1.15rem; font-weight: 800; color: #3E3F5F; }
.stat span { font-size: 0.72rem; color: #999; }
.stat-div { width: 1px; height: 32px; background: rgba(253,169,224,0.5); }

@media (max-width: 768px) {
  .hero { padding: 3rem 1.25rem; min-height: 300px; }
  .hero-title { font-size: 2.2rem; }
  .hero-stats { flex-wrap: wrap; gap: 0.75rem; }
}
</style>