<template>
  <header class="site-nav" :class="{ 'is-hidden': !isNavVisible }">
    <nav class="pill" aria-label="Navegação principal">
      <ul class="pill__links">
        <li v-for="link in links" :key="link.href">
          <a :href="link.href">{{ link.label }}</a>
        </li>
      </ul>

      <a href="#agendar" class="pill__cta">Agendar consulta</a>

      <button
        type="button"
        class="pill__burger"
        :class="{ 'is-open': menuOpen }"
        @click="toggleMenu"
        :aria-expanded="menuOpen"
        aria-controls="mobile-menu"
        :aria-label="menuOpen ? 'Fechar menu' : 'Abrir menu'"
      >
        <span></span><span></span><span></span>
      </button>
    </nav>

    <div
      id="mobile-menu"
      class="mobile-menu"
      :class="{ 'is-open': menuOpen }"
      role="dialog"
      aria-modal="true"
      :aria-hidden="!menuOpen"
    >
      <div class="mobile-menu__facet" aria-hidden="true"></div>
      <span class="mobile-menu__watermark" aria-hidden="true">CH</span>

      <button
        type="button"
        class="mobile-menu__close"
        @click="closeMenu"
        aria-label="Fechar menu"
        ref="overlayCloseRef"
      >
        <span></span><span></span>
      </button>

      <img
        src="/IMG/logo4.svg"
        alt="Cássio Honorato Odontologia"
        class="mobile-menu__logo"
        ref="overlayLogoRef"
      />

      <span class="mobile-menu__eyebrow" ref="overlayEyebrowRef">Menu</span>

      <nav class="mobile-menu__links" ref="overlayLinksRef">
        <a v-for="link in links" :key="link.href" :href="link.href" @click="closeMenu">
          {{ link.label }}
        </a>
      </nav>

      <a href="#agendar" class="mobile-menu__cta" @click="closeMenu" ref="overlayCtaRef">Agendar consulta</a>
    </div>
  </header>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue'
import gsap from 'gsap'

/**
 * Dependência: npm install gsap
 *
 * Comportamento do header:
 * - Nasce invisível no topo da página (deixa o masthead do Hero respirar sozinho).
 * - Ao rolar a página para baixo além de ~80px, a pill flutuante aparece.
 * - Ao voltar perto do topo, ela some de novo — sem lógica de direção,
 *   só posição de scroll, então não há flicker em scrolls rápidos.
 * - Desktop (>900px): pill com links + CTA, sem hambúrguer.
 * - Mobile (≤900px): pill vira botão circular (hambúrguer) que abre um
 *   overlay fullscreen com o logo real, links grandes e CTA, todos animados.
 */

const links = [
  { href: '#sobre', label: 'Sobre' },
  { href: '#servicos', label: 'Serviços' },
  { href: '#resultados', label: 'Resultados' },
  { href: '#consultorio', label: 'Consultorio' },
  { href: '#comochegar', label: 'Como Chegar' },
  { href: '#contato', label: 'Contato' }
]

const SHOW_THRESHOLD = 80

const overlayCloseRef = ref(null)
const overlayLogoRef = ref(null)
const overlayEyebrowRef = ref(null)
const overlayLinksRef = ref(null)
const overlayCtaRef = ref(null)

const menuOpen = ref(false)
const scrolledPastTop = ref(false)

// O nav fica visível se o usuário já rolou a página OU se o menu mobile
// está aberto (senão o botão de fechar "sumiria" junto com o header).
const isNavVisible = computed(() => scrolledPastTop.value || menuOpen.value)

const prefersReducedMotion =
  typeof window !== 'undefined' &&
  window.matchMedia('(prefers-reduced-motion: reduce)').matches

let scrollHandler = null

function animateOverlayOpen() {
  if (prefersReducedMotion) return
  const linkEls = overlayLinksRef.value?.querySelectorAll('a')

  gsap.fromTo(
    overlayCloseRef.value,
    { opacity: 0, scale: 0.6, rotate: -45 },
    { opacity: 1, scale: 1, rotate: 0, duration: 0.45, ease: 'back.out(2)' }
  )
  gsap.fromTo(
    overlayLogoRef.value,
    { opacity: 0, y: -16, scale: 0.9 },
    { opacity: 1, y: 0, scale: 1, duration: 0.5, ease: 'back.out(1.7)' }
  )
  gsap.fromTo(
    overlayEyebrowRef.value,
    { opacity: 0, y: -8 },
    { opacity: 1, y: 0, duration: 0.4, ease: 'power2.out', delay: 0.12 }
  )
  if (linkEls?.length) {
    gsap.fromTo(
      linkEls,
      { opacity: 0, y: 34, rotateX: -40 },
      {
        opacity: 1,
        y: 0,
        rotateX: 0,
        duration: 0.6,
        stagger: 0.08,
        ease: 'back.out(1.6)',
        delay: 0.18,
        transformOrigin: '50% 100%'
      }
    )
  }
  gsap.fromTo(
    overlayCtaRef.value,
    { opacity: 0, y: 18 },
    { opacity: 1, y: 0, duration: 0.5, ease: 'back.out(1.8)', delay: 0.5 }
  )
}

function openMenu() {
  menuOpen.value = true
}
function closeMenu() {
  menuOpen.value = false
}
function toggleMenu() {
  menuOpen.value ? closeMenu() : openMenu()
}

watch(menuOpen, (isOpen) => {
  document.body.style.overflow = isOpen ? 'hidden' : ''
  if (isOpen) nextTick(animateOverlayOpen)
})

function onKeydown(e) {
  if (e.key === 'Escape' && menuOpen.value) closeMenu()
}
function onResize() {
  if (window.innerWidth > 900 && menuOpen.value) closeMenu()
}

onMounted(() => {
  scrollHandler = () => {
    scrolledPastTop.value = window.scrollY > SHOW_THRESHOLD
  }
  window.addEventListener('scroll', scrollHandler, { passive: true })
  scrollHandler()

  window.addEventListener('keydown', onKeydown)
  window.addEventListener('resize', onResize)
})

onBeforeUnmount(() => {
  if (scrollHandler) window.removeEventListener('scroll', scrollHandler)
  window.removeEventListener('keydown', onKeydown)
  window.removeEventListener('resize', onResize)
  document.body.style.overflow = ''
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@500;600;700&display=swap');

.site-nav {
  --paper: #faf9f5;
  --paper-edge: #efe9db;
  --ink: #171208;
  --ink-dim: rgba(23, 18, 8, 0.62);
  --gold: #a9832a;
  --gold-bright: #cf9f3c;
  --gold-soft: #e4c674;
  --gold-deep: #6d4f16;

  position: fixed;
  top: clamp(14px, 3vw, 26px);
  left: 0;
  right: 0;
  z-index: 300;
  display: flex;
  justify-content: center;
  padding: 0 20px;
  pointer-events: none;
  font-family: 'Manrope', sans-serif;
}

/* ---------- Pill (barra flutuante) ---------- */
.pill {
  pointer-events: auto;
  display: flex;
  align-items: center;
  gap: 30px;
  padding: 12px 12px 12px 26px;
  border-radius: 999px;
  background: rgba(250, 249, 245, 0.85);
  backdrop-filter: blur(16px) saturate(140%);
  -webkit-backdrop-filter: blur(16px) saturate(140%);
  border: 1px solid rgba(169, 131, 42, 0.22);
  box-shadow: 0 14px 34px -16px rgba(23, 18, 8, 0.3);
  max-width: calc(100vw - 40px);
  transform: translateY(0);
  opacity: 1;
  transition: transform 0.5s cubic-bezier(0.22, 1, 0.36, 1), opacity 0.4s ease, box-shadow 0.35s ease;
}

.site-nav.is-hidden .pill {
  transform: translateY(-18px);
  opacity: 0;
  pointer-events: none;
}

.pill__links {
  display: flex;
  align-items: center;
  gap: 28px;
  list-style: none;
  margin: 0;
  padding: 0;
}
.pill__links a {
  position: relative;
  text-decoration: none;
  color: var(--ink-dim);
  font-family: 'Fraunces', serif;
  font-weight: 500;
  font-size: 0.92rem;
  letter-spacing: 0.01em;
  padding: 4px 0;
  transition: color 0.25s ease, letter-spacing 0.25s ease;
}
.pill__links a::after {
  content: '';
  position: absolute;
  left: 0;
  right: 100%;
  bottom: 0;
  height: 1.5px;
  background: var(--gold);
  transition: right 0.3s ease;
}
.pill__links a:hover {
  color: var(--gold-deep);
  letter-spacing: 0.02em;
}
.pill__links a:hover::after { right: 0; }

.pill__cta {
  flex-shrink: 0;
  white-space: nowrap;
  text-decoration: none;
  font-family: 'Manrope', sans-serif;
  font-size: 0.82rem;
  font-weight: 700;
  color: #231a08;
  padding: 11px 22px;
  border-radius: 999px;
  background: linear-gradient(120deg, var(--gold-soft), var(--gold) 55%, var(--gold-deep));
  box-shadow: 0 8px 20px -10px rgba(169, 131, 42, 0.5);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}
.pill__cta:hover {
  box-shadow: 0 12px 26px -10px rgba(169, 131, 42, 0.65);
  transform: translateY(-1px);
}

/* ---------- Botão hambúrguer (só existe visualmente no mobile) ---------- */
.pill__burger {
  display: none;
  position: relative;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: transparent;
  cursor: pointer;
  flex-shrink: 0;
}
.pill__burger span {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 19px;
  height: 2px;
  border-radius: 2px;
  background: var(--ink);
  transform: translate(-50%, -50%);
  transition: transform 0.35s ease, opacity 0.35s ease, background 0.3s ease;
}
.pill__burger span:nth-child(1) { transform: translate(-50%, -50%) translateY(-6px); }
.pill__burger span:nth-child(3) { transform: translate(-50%, -50%) translateY(6px); }
.pill__burger.is-open span:nth-child(1) { transform: translate(-50%, -50%) rotate(45deg); }
.pill__burger.is-open span:nth-child(2) { opacity: 0; }
.pill__burger.is-open span:nth-child(3) { transform: translate(-50%, -50%) rotate(-45deg); }

/* ---------- Overlay fullscreen (mobile) ---------- */
.mobile-menu {
  position: fixed;
  inset: 0;
  z-index: 200;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 26px;
  overflow: hidden;
  padding: 110px 32px 64px;
  background: radial-gradient(130% 140% at 50% -10%, #ffffff 0%, var(--paper) 46%, var(--paper-edge) 100%);
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition: opacity 0.4s ease, visibility 0s linear 0.4s;
}
.mobile-menu.is-open {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  transition: opacity 0.4s ease, visibility 0s linear 0s;
}

.mobile-menu__facet {
  position: absolute;
  width: 80vw;
  height: 80vw;
  max-width: 520px;
  max-height: 520px;
  top: -22%;
  right: -24%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.3), transparent 70%);
  filter: blur(54px);
  mix-blend-mode: multiply;
  pointer-events: none;
}

.mobile-menu__watermark {
  position: absolute;
  bottom: -6%;
  left: 50%;
  transform: translateX(-50%);
  font-family: 'Fraunces', serif;
  font-weight: 600;
  font-size: 42vw;
  line-height: 1;
  color: var(--gold);
  opacity: 0.05;
  pointer-events: none;
  white-space: nowrap;
}

.mobile-menu__close {
  position: absolute;
  top: clamp(18px, 4vw, 28px);
  right: clamp(18px, 4vw, 28px);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid rgba(169, 131, 42, 0.28);
  background: rgba(250, 249, 245, 0.7);
  cursor: pointer;
  opacity: 0;
  z-index: 5;
}
.mobile-menu__close span {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 17px;
  height: 2px;
  border-radius: 2px;
  background: var(--ink);
}
.mobile-menu__close span:nth-child(1) { transform: translate(-50%, -50%) rotate(45deg); }
.mobile-menu__close span:nth-child(2) { transform: translate(-50%, -50%) rotate(-45deg); }
.mobile-menu__close:hover {
  border-color: var(--gold);
  background: rgba(169, 131, 42, 0.1);
}

.mobile-menu__logo {
  position: relative;
  height: 46px;
  width: auto;
  opacity: 0;
  filter: drop-shadow(0 4px 10px rgba(23, 18, 8, 0.14));
}

.mobile-menu__eyebrow {
  position: relative;
  opacity: 0;
  font-size: 0.72rem;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: var(--gold-deep);
  font-weight: 700;
  margin-top: -10px;
}

.mobile-menu__links {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  perspective: 600px;
}
.mobile-menu__links a {
  position: relative;
  opacity: 0;
  text-decoration: none;
  font-family: 'Fraunces', serif;
  font-style: italic;
  font-weight: 600;
  font-size: clamp(2rem, 8.5vw, 2.7rem);
  color: var(--ink);
  padding: 10px 6px;
}
.mobile-menu__links a::after {
  content: '';
  position: absolute;
  left: 50%;
  bottom: 6px;
  width: 0;
  height: 2px;
  background: var(--gold);
  transition: width 0.3s ease, left 0.3s ease;
}
.mobile-menu__links a:hover,
.mobile-menu__links a:active {
  color: var(--gold-deep);
}
.mobile-menu__links a:hover::after,
.mobile-menu__links a:active::after {
  width: 56%;
  left: 22%;
}

.mobile-menu__cta {
  position: relative;
  opacity: 0;
  text-decoration: none;
  font-family: 'Manrope', sans-serif;
  font-weight: 700;
  font-size: 0.92rem;
  color: #231a08;
  padding: 15px 34px;
  border-radius: 999px;
  background: linear-gradient(120deg, var(--gold-soft), var(--gold) 55%, var(--gold-deep));
  box-shadow: 0 12px 28px -12px rgba(169, 131, 42, 0.5);
  margin-top: 10px;
}

/* ---------- Responsivo ---------- */
@media (max-width: 900px) {
  .pill__links,
  .pill__cta {
    display: none;
  }
  .pill {
    gap: 0;
    padding: 0;
    width: 52px;
    height: 52px;
    justify-content: center;
  }
  .pill__burger {
    display: block;
  }
}

@media (min-width: 901px) {
  .mobile-menu {
    display: none;
  }
}

/* ---------- Acessibilidade / reduced motion ---------- */
@media (prefers-reduced-motion: reduce) {
  .site-nav,
  .pill,
  .mobile-menu,
  .mobile-menu__close,
  .mobile-menu__logo,
  .mobile-menu__eyebrow,
  .mobile-menu__links a,
  .mobile-menu__cta {
    transition: none !important;
  }
  .site-nav.is-hidden .pill {
    opacity: 1 !important;
    transform: none !important;
  }
  .mobile-menu.is-open .mobile-menu__close,
  .mobile-menu.is-open .mobile-menu__logo,
  .mobile-menu.is-open .mobile-menu__eyebrow,
  .mobile-menu.is-open .mobile-menu__links a,
  .mobile-menu.is-open .mobile-menu__cta {
    opacity: 1 !important;
  }
  .pill__burger span {
    transition: none !important;
  }
}
</style>