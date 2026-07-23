<template>
  <section class="results" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="results__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="results__inner">
      <div class="results__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Resultados reais</p>
        </div>
        <h2 class="headline">Sorrisos <span class="word--gold">transformados</span></h2>
        <p class="subtext">
          Arraste o marcador para comparar cada caso — antes e depois do tratamento.
        </p>
      </div>

      <div class="cases">
        <article v-for="(c, i) in cases" :key="c.id" class="case" :style="{ transitionDelay: (i % 3) * 0.08 + 's' }">
          <div class="case__text">
            <span class="case__index">Caso {{ String(i + 1).padStart(2, '0') }}</span>
            <p class="case__tag">{{ c.tag }}</p>
            <h3 class="case__title">{{ c.title }}</h3>
            <p class="case__desc">{{ c.desc }}</p>
            <ul class="case__meta">
              <li><span>Tratamento</span><strong>{{ c.treatment }}</strong></li>
              <li><span>Duração</span><strong>{{ c.duration }}</strong></li>
            </ul>
          </div>

          <div class="compare__frame">
            <img-comparison-slider class="compare__slider" value="50" @slide-start="interacted[i] = true">
              <img slot="first" :src="c.beforeImg" :alt="`Antes — ${c.title}`" loading="lazy">
              <img slot="second" :src="c.afterImg" :alt="`Depois — ${c.title}`" loading="lazy">
              <div slot="handle" class="compare__handle">
                <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
                  <path d="M8 7l-5 5 5 5M16 7l5 5-5 5" />
                </svg>
              </div>
            </img-comparison-slider>

            <span class="compare__label compare__label--before">Antes</span>
            <span class="compare__label compare__label--after">Depois</span>
            <p v-if="!interacted[i]" class="compare__hint">Arraste para comparar</p>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive, onMounted, onBeforeUnmount, ref } from 'vue'
import 'img-comparison-slider'

/**
 * Slider antes/depois com img-comparison-slider (npm), não é implementação própria.
 * A lib já resolve mouse, touch e teclado nativamente, sem conflito com o
 * drag nativo de imagem — é isso que corrigia o bug de "só funciona no teclado".
 *
 * Troque `beforeImg` / `afterImg` pelas fotos reais de cada caso.
 * Recomendado: fotos recortadas focando na boca/sorriso (proporção horizontal,
 * tipo 16:9), mesmo ângulo e iluminação entre antes/depois.
 */

const sectionRef = ref(null)
const visible = ref(false)

const cases = [
  {
    id: 'clareamento',
    tag: 'Estética · Clareamento',
    title: 'Clareamento em 2 sessões',
    desc: 'Dentes escurecidos por anos de café e vinho, recuperando tom natural sem sensibilizar o esmalte.',
    treatment: 'Clareamento a laser',
    duration: '2 sessões',
    beforeImg: '/IMG/t1.png',
    afterImg: '/IMG/t2.png'
  },
  {
    id: 'facetas',
    tag: 'Estética · Facetas',
    title: 'Facetas de porcelana',
    desc: 'Correção de forma, cor e pequeno desalinhamento aparente com 6 facetas no arco superior.',
    treatment: '6 facetas de porcelana',
    duration: '3 sessões',
    beforeImg: '/IMG/t1.png',
    afterImg: '/IMG/t2.png'
  },
  {
    id: 'lentes',
    tag: 'Estética · Lentes em resina',
    title: 'Fechamento de diastema',
    desc: 'Espaço entre os dentes frontais fechado com lentes em resina, mantendo a naturalidade do sorriso.',
    treatment: 'Lentes em resina',
    duration: '1 sessão',
    beforeImg: '/IMG/t1.png',
    afterImg: '/IMG/t2.png'
  },
  {
    id: 'implante',
    tag: 'Cirúrgica · Implante',
    title: 'Reposição de dente perdido',
    desc: 'Substituição de um dente perdido há anos por implante de titânio e coroa personalizada.',
    treatment: 'Implante + coroa',
    duration: '4 meses (em etapas)',
    beforeImg: '/IMG/t1.png',
    afterImg: '/IMG/t2.png'
  },
  {
    id: 'alinhador',
    tag: 'Ortodontia · Alinhador invisível',
    title: 'Correção de apinhamento leve',
    desc: 'Pequeno apinhamento nos dentes inferiores corrigido de forma discreta, sem aparelho fixo.',
    treatment: 'Alinhadores invisíveis',
    duration: '8 meses',
    beforeImg: '/IMG/t1.png',
    afterImg: '/IMG/t2.png'
  }
]

const interacted = reactive(cases.map(() => false))

let observer = null

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (prefersReducedMotion || !sectionRef.value) {
    visible.value = true
    return
  }

  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        visible.value = true
        observer.disconnect()
      }
    },
    { threshold: 0.1 }
  )
  observer.observe(sectionRef.value)
})

onBeforeUnmount(() => {
  observer?.disconnect()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.results {
  --paper: #faf9f5;
  --ink: #171208;
  --ink-dim: rgba(23, 18, 8, 0.6);
  --gold: #a9832a;
  --gold-bright: #cf9f3c;
  --gold-soft: #e4c674;
  --gold-deep: #6d4f16;
  --gold-line: rgba(169, 131, 42, 0.28);

  position: relative;
  overflow: hidden;
  background: #f9f8f3;
  color: var(--ink);
  font-family: 'Manrope', sans-serif;
  padding: clamp(64px, 9vw, 120px) clamp(20px, 6vw, 64px);
}

.results__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 30vw; height: 30vw; max-width: 420px; max-height: 420px;
  top: -8%; left: -10%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.16), transparent 70%);
  filter: blur(50px);
  mix-blend-mode: multiply;
}

.results__inner { position: relative; z-index: 2; max-width: 1080px; margin: 0 auto; }

.results__head { text-align: center; max-width: 620px; margin: 0 auto clamp(36px, 5vw, 52px); }
.eyebrow-row { display: flex; align-items: center; justify-content: center; gap: 10px; margin-bottom: 14px; }
.eyebrow-mark { color: var(--gold); font-size: .85rem; }
.eyebrow { font-size: .78rem; letter-spacing: .22em; text-transform: uppercase; color: var(--gold-deep); margin: 0; font-weight: 600; }
.headline { font-family: 'Fraunces', serif; font-weight: 600; font-size: clamp(1.9rem, 3.6vw, 2.9rem); line-height: 1.15; margin: 0 0 16px; }
.word--gold {
  background: linear-gradient(100deg, var(--gold-deep) 0%, var(--gold-soft) 35%, var(--gold-bright) 55%, var(--gold-soft) 75%, var(--gold-deep) 100%);
  background-size: 220% 100%;
  -webkit-background-clip: text; background-clip: text; color: transparent; font-style: italic;
}
.subtext { font-size: clamp(1rem, 1.2vw, 1.08rem); line-height: 1.65; color: var(--ink-dim); margin: 0; }

.cases { display: flex; flex-direction: column; gap: clamp(28px, 4vw, 44px); }

.case {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: clamp(24px, 3.5vw, 48px);
  align-items: center;
  padding-bottom: clamp(28px, 4vw, 44px);
  border-bottom: 1px solid rgba(23, 18, 8, .08);
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.case:last-child { border-bottom: none; padding-bottom: 0; }
.results.is-visible .case { opacity: 1; transform: none; }

.case__text { display: flex; flex-direction: column; }
.case__index { font-family: 'Fraunces', serif; font-style: italic; font-size: .82rem; color: var(--gold); margin-bottom: 6px; }
.case__tag { font-size: .74rem; letter-spacing: .06em; text-transform: uppercase; color: var(--gold-deep); font-weight: 700; margin: 0 0 8px; }
.case__title { font-family: 'Fraunces', serif; font-weight: 600; font-size: clamp(1.2rem, 1.6vw, 1.45rem); margin: 0 0 8px; }
.case__desc { font-size: .9rem; line-height: 1.6; color: var(--ink-dim); margin: 0 0 16px; }

.case__meta { list-style: none; margin: 0; padding: 14px 0 0; border-top: 1px solid rgba(23,18,8,.08); display: flex; flex-direction: column; gap: 7px; }
.case__meta li { display: flex; justify-content: space-between; gap: 12px; font-size: .8rem; }
.case__meta span { color: var(--ink-dim); }
.case__meta strong { color: var(--ink); font-weight: 700; text-align: right; }

/* --- Slider antes/depois: agora horizontal (16:9), foco no sorriso --- */
.compare__frame {
  position: relative; /* <- adicionar isso */
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 20px 40px -22px rgba(23, 18, 8, .32);
  border: 1px solid rgba(23, 18, 8, .08);
  background: var(--paper);
}

.compare__slider {
  width: 100%;
  height: 100%;
  display: block;
  --divider-width: 2px;
  --divider-color: rgba(250, 249, 245, .9);
  --default-handle-opacity: 0;
}
.compare__slider img {
  display: block !important;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.compare__handle {
  width: 34px; height: 34px; border-radius: 50%;
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  color: #241a08;
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 6px 14px -4px rgba(23,18,8,.4);
}

.compare__label {
  position: absolute; top: 12px;
  font-size: .68rem; font-weight: 700; letter-spacing: .06em; text-transform: uppercase;
  padding: 5px 10px; border-radius: 999px;
  background: rgba(23, 18, 8, .55); color: #fbf3de;
  backdrop-filter: blur(2px);
  pointer-events: none;
  z-index: 2;
}
.compare__label--before { left: 12px; }
.compare__label--after { right: 12px; }

.compare__hint {
  position: absolute; bottom: 12px; left: 50%; transform: translateX(-50%);
  margin: 0; font-size: .72rem; font-weight: 600; letter-spacing: .02em;
  color: #fbf3de; background: rgba(23, 18, 8, .55);
  padding: 5px 12px; border-radius: 999px;
  pointer-events: none;
  z-index: 2;
  animation: hint-pulse 1.8s ease-in-out infinite;
}
@keyframes hint-pulse {
  0%, 100% { opacity: .85; }
  50% { opacity: 1; }
}

@media (max-width: 860px) {
  .case { grid-template-columns: 1fr; gap: 14px; }
  .compare__frame { aspect-ratio: 16 / 9; border-radius: 12px; }
  .case__text { order: 2; }
  .compare__frame { order: 1; }
  .case__desc { margin-bottom: 12px; }
  .case__meta { padding-top: 10px; gap: 5px; }
  .case__meta li { font-size: .76rem; }
}

@media (prefers-reduced-motion: reduce) {
  .case { opacity: 1 !important; transform: none !important; transition: none !important; }
  .compare__hint { animation: none; }
}
</style>