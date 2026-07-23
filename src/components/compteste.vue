<template>
  <section class="hero" ref="heroRef" @mousemove="onHeroMouseMove" @mouseleave="onHeroMouseLeave">
    <div class="hero__ambient" aria-hidden="true">
      <div class="facet facet--a"></div>
      <div class="facet facet--b"></div>
    </div>

    <div class="masthead">
      <div class="masthead__logo-wrap" ref="logoWrapRef">
        <span class="logo-plate" aria-hidden="true"></span>
        <img src="/IMG/logo4.svg" alt="Cássio Honorato Odontologia" class="masthead__logo" ref="logoRef" />
        <span class="logo-shine" ref="logoShineRef" aria-hidden="true"></span>
      </div>
      <span class="cro-badge" ref="croBadgeRef">CIRURGIÃO-DENTISTA · CRO-6805</span>
    </div>

    <div class="hero__inner">
      <div class="hero__content">
        <div class="hero__eyebrow-row" ref="eyebrowRowRef">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="hero__eyebrow" ref="eyebrowRef">Odontologia de alto padrão · Pedro II</p>
        </div>

        <h1 class="hero__headline" ref="headlineRef">
          <span class="word">Cada</span>
          <span class="word">sorriso</span><br />
          <span class="word word--gold">lapidado</span>
          <span class="word">como</span>
          <span class="word word--gold">joia.</span>
          <svg class="word-underline" viewBox="0 0 200 10" preserveAspectRatio="none" aria-hidden="true">
            <path class="underline-fringe" d="M2 6 Q 50 2 100 5 T 198 6" />
            <path class="underline-gold" d="M2 5 Q 50 1 100 4 T 198 5" />
          </svg>
        </h1>

        <p class="hero__subtext" ref="subtextRef">
          Excelência clínica e um olhar de ourives para cada detalhe do seu sorriso.
        </p>

        <div class="chips" ref="chipsRef">
          <span class="chip" v-for="c in specialties" :key="c.label">
            <span class="chip-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" v-html="c.icon"></svg>
            </span>
            {{ c.label }}
          </span>
        </div>

        <div class="hero__cta" ref="ctaRef">
          <a href="#agendar" class="btn btn--primary" ref="btnPrimaryRef" @click="onAgendarClick">
            <span class="btn-inner">Agendar consulta</span>
          </a>
          <a href="#sobre" class="btn btn--ghost" ref="btnGhostRef">
            <span class="btn-inner">Conhecer a clínica</span>
          </a>
        </div>
      </div>

      <div class="hero__visual">
        <div class="lottie-stage" ref="stageRef">
          <div class="prism-ring" aria-hidden="true"></div>
          <div class="lottie-glow"></div>
          <div class="lottie-box" ref="lottieBoxRef"></div>
          <div class="pedestal" aria-hidden="true"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import gsap from 'gsap'
import lottie from 'lottie-web'

/**
 * Dependências:
 *   npm install gsap lottie-web
 *
 * Lottie: coloque seu arquivo exportado em public/IMG/dente.json
 * Logo: public/IMG/logo4.svg
 */

const heroRef = ref(null)
const croBadgeRef = ref(null)
const logoWrapRef = ref(null)
const logoRef = ref(null)
const logoShineRef = ref(null)
const eyebrowRowRef = ref(null)
const eyebrowRef = ref(null)
const headlineRef = ref(null)
const subtextRef = ref(null)
const chipsRef = ref(null)
const ctaRef = ref(null)
const stageRef = ref(null)
const lottieBoxRef = ref(null)
const btnPrimaryRef = ref(null)
const btnGhostRef = ref(null)

const specialties = [
  {
    label: 'Cirurgia Oral Menor',
    icon: '<path d="M7 3c3 0 4 2 5 2s2-2 5-2c2 0 3 2 3 5 0 6-3 8-3 12 0 2-1 3-2 3s-2-2-2-4-1-3-1-3-1 1-1 3-1 4-2 4-2-1-2-3c0-4-3-6-3-12 0-3 1-5 3-5Z"/>'
  },
  {
    label: 'Ortodontia',
    icon: '<path d="M4 12h16M4 12a4 4 0 0 1 4-4h1M4 12a4 4 0 0 0 4 4h1M20 12a4 4 0 0 0-4-4h-1M20 12a4 4 0 0 1-4 4h-1"/><circle cx="9" cy="8" r="1.3" fill="currentColor" stroke="none"/><circle cx="15" cy="16" r="1.3" fill="currentColor" stroke="none"/>'
  },
  {
    label: 'Lentes em Resina',
    icon: '<path d="M6 4h12l1 5-7 11L5 9Z"/><path d="M9 4l3 5 3-5M6 9h12"/>'
  }
]

const prefersReducedMotion =
  typeof window !== 'undefined' &&
  window.matchMedia('(prefers-reduced-motion: reduce)').matches

let rafId = null
let magneticCleanups = []
let lottieAnim = null

function onHeroMouseMove(e) {
  if (prefersReducedMotion || !stageRef.value) return

  const rect = heroRef.value.getBoundingClientRect()
  const x = (e.clientX - rect.left) / rect.width - 0.5
  const y = (e.clientY - rect.top) / rect.height - 0.5

  if (rafId) cancelAnimationFrame(rafId)
  rafId = requestAnimationFrame(() => {
    gsap.to(stageRef.value, {
      rotateY: x * 14,
      rotateX: -y * 9,
      x: x * 8,
      y: y * 6,
      duration: 0.7,
      ease: 'power3.out',
      overwrite: 'auto'
    })
  })
}

function onHeroMouseLeave() {
  if (prefersReducedMotion || !stageRef.value) return
  gsap.to(stageRef.value, { rotateY: 0, rotateX: 0, x: 0, y: 0, duration: 1, ease: 'power3.out' })
}

function onAgendarClick(e) {
  // Em produção, troque isto pela navegação real (rota de agendamento, WhatsApp, etc).
  e.preventDefault()
  const rect = e.currentTarget.getBoundingClientRect()
  const cx = rect.left + rect.width / 2
  const cy = rect.top + rect.height / 2

  for (let i = 0; i < 10; i++) {
    const p = document.createElement('span')
    p.className = 'burst-particle'
    const angle = (Math.PI * 2 * i) / 10 + Math.random() * 0.3
    const dist = 32 + Math.random() * 36
    p.style.left = cx + 'px'
    p.style.top = cy + 'px'
    p.style.setProperty('--tx', Math.cos(angle) * dist + 'px')
    p.style.setProperty('--ty', Math.sin(angle) * dist + 'px')
    document.body.appendChild(p)
    p.addEventListener('animationend', () => p.remove())
  }
}

function setupMagneticButton(btn) {
  if (!btn || prefersReducedMotion) return
  const moveX = gsap.quickTo(btn, 'x', { duration: 0.4, ease: 'power3.out' })
  const moveY = gsap.quickTo(btn, 'y', { duration: 0.4, ease: 'power3.out' })

  const handleMove = (e) => {
    const rect = btn.getBoundingClientRect()
    const relX = e.clientX - rect.left - rect.width / 2
    const relY = e.clientY - rect.top - rect.height / 2
    moveX(relX * 0.3)
    moveY(relY * 0.3)
  }
  const handleLeave = () => { moveX(0); moveY(0) }

  btn.addEventListener('mousemove', handleMove)
  btn.addEventListener('mouseleave', handleLeave)
  magneticCleanups.push(() => {
    btn.removeEventListener('mousemove', handleMove)
    btn.removeEventListener('mouseleave', handleLeave)
  })
}

onMounted(() => {
  try {
    lottieAnim = lottie.loadAnimation({
      container: lottieBoxRef.value,
      renderer: 'svg',
      loop: true,
      autoplay: true,
      path: '/IMG/dente.json'
    })
  } catch (err) {
    console.warn('Lottie ainda não configurado:', err)
  }

  setupMagneticButton(btnPrimaryRef.value)
  setupMagneticButton(btnGhostRef.value)

  if (prefersReducedMotion) return

  const words = headlineRef.value.querySelectorAll('.word')

  gsap.set(logoShineRef.value, { xPercent: -220, skewX: -18 })

  const tl = gsap.timeline({ defaults: { ease: 'power4.out' } })

  tl.fromTo(logoWrapRef.value, { opacity: 0, y: -14, scale: 0.88 }, { opacity: 1, y: 0, scale: 1, duration: 0.6, ease: 'back.out(1.8)' })
    .fromTo(logoRef.value, { opacity: 0 }, { opacity: 1, duration: 0.6, ease: 'power2.out' }, '<')
    .to(logoShineRef.value, { xPercent: 220, duration: 0.8, ease: 'power2.inOut' }, '-=0.1')
    .fromTo(croBadgeRef.value, { opacity: 0, y: -12, scale: 0.9 }, { opacity: 1, y: 0, scale: 1, duration: 0.5, ease: 'back.out(2)' }, '-=0.5')
    .fromTo(eyebrowRowRef.value.children, { opacity: 0, x: -10 }, { opacity: 1, x: 0, duration: 0.45, stagger: 0.08 }, '-=0.15')
    .fromTo(
      words,
      { opacity: 0, y: 44, rotateX: -50, rotateZ: -3 },
      { opacity: 1, y: 0, rotateX: 0, rotateZ: 0, duration: 0.75, ease: 'back.out(1.5)', stagger: 0.07 },
      '-=0.2'
    )
    .fromTo(subtextRef.value, { opacity: 0, y: 16 }, { opacity: 1, y: 0, duration: 0.55 }, '-=0.35')
    .fromTo(
      chipsRef.value.children,
      { opacity: 0, y: 14, scale: 0.85, rotate: -8 },
      { opacity: 1, y: 0, scale: 1, rotate: 0, duration: 0.5, ease: 'back.out(1.8)', stagger: 0.08 },
      '-=0.25'
    )
    .fromTo(
      btnPrimaryRef.value,
      { opacity: 0, x: -24, rotate: -4 },
      { opacity: 1, x: 0, rotate: 0, duration: 0.55, ease: 'back.out(1.9)' },
      '-=0.2'
    )
    .fromTo(
      btnGhostRef.value,
      { opacity: 0, x: 24, rotate: 4 },
      { opacity: 1, x: 0, rotate: 0, duration: 0.55, ease: 'back.out(1.9)' },
      '<0.06'
    )
    .fromTo(
      stageRef.value,
      { opacity: 0, scale: 0.7, rotateY: -50, rotateZ: 5 },
      { opacity: 1, scale: 1, rotateY: 0, rotateZ: 0, duration: 1.1, ease: 'expo.out' },
      '-=1.1'
    )
})

onBeforeUnmount(() => {
  if (rafId) cancelAnimationFrame(rafId)
  magneticCleanups.forEach((cleanup) => cleanup())
  if (lottieAnim) lottieAnim.destroy()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.hero {
  --paper: #faf9f5;
  --paper-edge: #efe9db;
  --ink: #171208;
  --ink-dim: rgba(23, 18, 8, 0.6);
  --gold: #a9832a;
  --gold-bright: #cf9f3c;
  --gold-soft: #e4c674;
  --gold-deep: #6d4f16;
  --gold-line: rgba(169, 131, 42, 0.32);

  position: relative;
  overflow: hidden;
  min-height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: #F9F8F3;
  color: var(--ink);
  font-family: 'Manrope', sans-serif;
  padding: clamp(20px, 4vw, 44px) clamp(20px, 6vw, 64px);
}

.hero__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }

.facet {
  position: absolute;
  background: linear-gradient(135deg, rgba(169, 131, 42, 0.35), rgba(207, 159, 60, 0.12) 60%, transparent 82%);
  filter: blur(50px);
  mix-blend-mode: multiply;
}
.facet--a {
  width: 40vw; height: 40vw; max-width: 520px; max-height: 520px; top: -20%; right: -12%;
  clip-path: polygon(20% 0%, 100% 15%, 85% 100%, 0% 75%);
  opacity: .35; animation: facet-float-a 30s ease-in-out infinite;
}
.facet--b {
  width: 24vw; height: 24vw; max-width: 340px; max-height: 340px; bottom: -10%; left: -10%;
  clip-path: polygon(0% 20%, 80% 0%, 100% 90%, 15% 100%);
  opacity: .2; animation: facet-float-b 36s ease-in-out infinite;
}
@keyframes facet-float-a { 0%,100% { transform: translate(0,0) rotate(0deg); } 50% { transform: translate(-3%,4%) rotate(6deg); } }
@keyframes facet-float-b { 0%,100% { transform: translate(0,0) rotate(0deg); } 50% { transform: translate(3%,-3%) rotate(-7deg); } }

.masthead {
  position: relative; z-index: 3; display: flex; align-items: center; justify-content: space-between;
  flex-wrap: wrap; gap: 18px; margin-bottom: clamp(24px, 4.5vw, 44px);
}
.masthead__logo-wrap { position: relative; display: inline-flex; align-items: center; line-height: 0; flex: 0 0 auto; max-width: 100%; }
.logo-plate {
  position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%);
  width: 120%; height: 220%; border-radius: 18px;
  background: radial-gradient(ellipse at center, #ffffff 0%, rgba(255,255,255,.7) 55%, transparent 78%);
  z-index: -1;
}
.masthead__logo {
  height: clamp(56px, 7vw, 96px); width: auto; max-width: 100%; display: block; opacity: 0;
  filter: drop-shadow(0 4px 10px rgba(23, 18, 8, 0.16)) drop-shadow(0 0 18px rgba(207, 159, 60, 0.3));
}
.logo-shine {
  position: absolute; top: -30%; left: 0; width: 45%; height: 160%;
  background: linear-gradient(115deg, transparent, rgba(228, 198, 116, 0.9), transparent);
  mix-blend-mode: multiply; pointer-events: none;
}

.cro-badge {
  display: inline-flex; align-items: center; gap: 8px;
  padding: 9px 16px; border: 1px solid var(--gold-line); border-radius: 999px;
  font-size: .7rem; letter-spacing: .08em; color: var(--gold-deep); font-weight: 700;
  opacity: 0; white-space: nowrap; position: relative; background: rgba(169, 131, 42, .06);
}
.cro-badge::before {
  content: ''; width: 6px; height: 6px; border-radius: 50%; background: var(--gold);
  animation: pulse-dot 2.4s ease-in-out infinite;
}
@keyframes pulse-dot { 0%,100% { box-shadow: 0 0 0 0 rgba(169,131,42,.4); } 50% { box-shadow: 0 0 0 5px rgba(169,131,42,0); } }

.hero__inner {
  position: relative; z-index: 2; display: grid; grid-template-columns: 1.05fr .95fr;
  align-items: center; gap: clamp(24px,4vw,52px); max-width: 1240px; margin: 0 auto; width: 100%;
}

.hero__eyebrow-row { display: flex; align-items: center; gap: 10px; margin-bottom: 14px; }
.eyebrow-mark { color: var(--gold); font-size: .85rem; opacity: 0; }
.hero__eyebrow { font-size:.78rem; letter-spacing:.22em; text-transform:uppercase; color: var(--gold-deep); margin:0; opacity:0; font-weight: 600; }

.hero__headline { font-family:'Fraunces',serif; font-weight:600; font-size: clamp(2.2rem,4.4vw,3.6rem); line-height:1.06; margin:0 0 18px; perspective:700px; color: var(--ink); }
.hero__headline .word { display:inline-block; opacity:0; margin-right:.28em; transition: transform .35s ease; }
.hero__headline .word:hover { transform: translateY(-4px) rotateZ(-1.5deg); }
.hero__headline .word--gold {
  background: linear-gradient(100deg, var(--gold-deep) 0%, var(--gold-soft) 35%, var(--gold-bright) 55%, var(--gold-soft) 75%, var(--gold-deep) 100%);
  background-size: 220% 100%;
  -webkit-background-clip:text; background-clip:text; color:transparent; font-style:italic;
  animation: gold-shimmer 7s ease-in-out infinite;
}
@keyframes gold-shimmer { 0%,100% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } }

.word-underline { display:block; width:150px; max-width:55%; height:10px; margin-top:-2px; overflow: visible; }
.underline-gold { stroke: var(--gold); stroke-width: 2.5; fill:none; stroke-dasharray: 200; stroke-dashoffset: 200; animation: draw-underline 1s 1.7s ease forwards; }
.underline-fringe { stroke: rgba(122,92,148,.4); stroke-width: 1.4; fill:none; opacity: .5; stroke-dasharray: 200; stroke-dashoffset: 200; animation: draw-underline 1.1s 1.8s ease forwards; }
@keyframes draw-underline { to { stroke-dashoffset: 0; } }

.hero__subtext { font-size: clamp(.98rem,1.2vw,1.08rem); line-height:1.6; color: var(--ink-dim); max-width:440px; margin:0 0 22px; opacity:0; }

.chips { display:flex; flex-wrap:wrap; gap:10px; margin-bottom: 26px; }
.chip {
  display:inline-flex; align-items:center; gap:9px; padding: 8px 16px 8px 8px;
  border-radius: 999px; border: 1px solid rgba(23,18,8,.12); background: rgba(169,131,42,.05);
  font-size: .78rem; font-weight: 600; color: var(--ink); opacity: 0; transition: border-color .25s ease, background .25s ease, transform .25s ease;
}
.chip:hover { border-color: var(--gold); background: rgba(169,131,42,.12); transform: translateY(-2px); }
.chip-icon {
  display:inline-flex; align-items:center; justify-content:center; width: 24px; height: 24px;
  border-radius: 50%; border: 1px solid var(--gold-line); background: rgba(169,131,42,.07); flex-shrink: 0;
}
.chip-icon svg { width:13px; height:13px; stroke: var(--gold-deep); }

.hero__cta { display:flex; flex-wrap:wrap; gap:16px; }
.btn {
  position: relative; display:inline-flex; align-items:center; justify-content:center;
  padding:15px 30px; font-size:.92rem; font-weight:700; letter-spacing:.02em; text-decoration:none;
  border-radius:999px; overflow:visible; transition: box-shadow .35s ease; opacity:0; will-change: transform;
}
.btn-inner { position:relative; z-index:2; }
.btn--primary { color:#231a08; background: linear-gradient(120deg, var(--gold-soft), var(--gold) 55%, var(--gold-deep)); box-shadow: 0 10px 26px -10px rgba(169,131,42,.45); overflow:hidden; }
.btn--primary::before {
  content:''; position:absolute; inset:0;
  background: linear-gradient(120deg, transparent 30%, rgba(255,255,255,.75) 50%, transparent 70%);
  transform: translateX(-120%); transition: transform .7s ease;
}
.btn--primary:hover::before { transform: translateX(120%); }
.btn--primary:hover { box-shadow: 0 16px 34px -10px rgba(169,131,42,.6); }
.btn--ghost { color: var(--ink); border: 1.5px solid rgba(23,18,8,.2); background: transparent; }
.btn--ghost:hover { border-color: var(--gold); background: rgba(169,131,42,.08); }

.hero__visual { display:flex; align-items:center; justify-content:center; perspective:1200px; position: relative; z-index: 2; }
.lottie-stage { position:relative; width: clamp(200px,22vw,300px); aspect-ratio: 1/1.05; }
.lottie-glow {
  position:absolute; inset: -18%; border-radius: 50%;
  background: radial-gradient(circle, rgba(207,159,60,.26), transparent 68%);
  filter: blur(14px); animation: halo-pulse 5.5s ease-in-out infinite;
  mix-blend-mode: multiply;
}
@keyframes halo-pulse { 0%,100%{opacity:.5;transform:scale(.96);} 50%{opacity:.9;transform:scale(1.05);} }

.prism-ring {
  position: absolute; inset: -8%; border-radius: 50%;
  background: conic-gradient(from 0deg, transparent 0deg, rgba(207,159,60,.5) 35deg, transparent 90deg, rgba(169,131,42,.4) 165deg, transparent 225deg, rgba(122,92,148,.22) 280deg, transparent 340deg, transparent 360deg);
  filter: blur(3px);
  animation: prism-spin 18s linear infinite;
  mix-blend-mode: multiply;
  -webkit-mask: radial-gradient(closest-side, transparent 76%, black 79%, black 91%, transparent 94%);
  mask: radial-gradient(closest-side, transparent 76%, black 79%, black 91%, transparent 94%);
}
@keyframes prism-spin { to { transform: rotate(360deg); } }

.pedestal {
  position: absolute; left: 50%; bottom: 4%; width: 58%; height: 10%;
  transform: translateX(-50%);
  background: radial-gradient(ellipse at center, rgba(169,131,42,.28), transparent 72%);
  filter: blur(6px);
  mix-blend-mode: multiply;
}

.lottie-box { position: relative; width: 100%; height: 100%; z-index: 1; }

@media (max-width: 900px) {
  .hero { min-height: auto; }
  .hero__inner { grid-template-columns: 1fr; text-align:center; }
  .hero__eyebrow-row { justify-content: center; }
  .hero__subtext { margin-left:auto; margin-right:auto; }
  .chips, .hero__cta { justify-content:center; }
  .hero__visual { order:-1; margin-bottom: 8px; }
  .lottie-stage { width: clamp(170px,46vw,220px); }
  .masthead { justify-content: center; text-align: center; }
}

@media (prefers-reduced-motion: reduce) {
  .cro-badge, .masthead__logo, .eyebrow-mark, .hero__eyebrow, .hero__headline .word, .hero__subtext, .chip, .btn, .lottie-stage {
    opacity: 1 !important;
  }
  .hero__headline .word, .chip, .btn, .masthead__logo, .cro-badge {
    transform: none !important;
  }
  .facet, .prism-ring, .lottie-glow, .underline-gold, .underline-fringe, .cro-badge::before, .word--gold, .logo-shine {
    animation: none !important;
  }
  .word--gold { background-position: 50% 50% !important; }
  .logo-shine { display: none; }
}
</style>

<!--
  Estilo global (sem "scoped") necessário porque a explosão do botão é criada
  dinamicamente via JS e anexada ao <body>, fora da árvore do componente.
-->
<style>
.burst-particle {
  position: fixed;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #c6a133;
  pointer-events: none;
  z-index: 60;
  animation: burst-fly 0.7s ease-out forwards;
}
@keyframes burst-fly {
  to { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
}
</style>