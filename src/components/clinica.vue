<template>
  <section class="about" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="about__ambient" aria-hidden="true">
      <div class="facet facet--a"></div>
      <div class="facet facet--b"></div>
    </div>

    <div class="about__inner">
      <div class="about__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Conheça a clínica</p>
        </div>
        <h2 class="headline">Cuidado que carrega <span class="word--gold">nome e rosto</span></h2>
        <p class="subtext">
          Por trás de cada sorriso transformado existe uma pessoa dedicada a fazer isso com técnica e carinho.
        </p>
      </div>

      <!-- Perfil -->
      <div class="profile">
        <div class="profile__photo">
          <div class="photo-frame">
            <img src="/IMG/drCassio.png" alt="Dra. Cássio Honorato, cirurgiã-dentista" loading="lazy" />
          </div>
          <span class="chip chip--top">
            <svg viewBox="0 0 24 24" width="14" height="14" fill="currentColor"><path d="M12 3.5l2.6 5.4 5.9.8-4.3 4.1 1 5.9L12 16.9 6.8 19.7l1-5.9L3.5 9.7l5.9-.8L12 3.5z" /></svg>
            4.9 de média nas avaliações
          </span>
          <span class="chip chip--bottom">
            <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 21s7-6.4 7-12a7 7 0 1 0-14 0c0 5.6 7 12 7 12Z" /><circle cx="12" cy="9" r="2.4" /></svg>
            9 anos cuidando de sorrisos
          </span>
          <div class="photo-ring" aria-hidden="true"></div>
        </div>

        <div class="profile__text">
          <p class="profile__role">Dentista</p>
          <h3 class="profile__name">Dr. Cássio Honorato</h3>
          <p class="profile__cro">CRO-PI 4821 · Especialista em Odontologia Estética e Implantodontia</p>

          <p class="profile__bio">
            Formada pela UFPI em 2014, a Dr. Cássio Honorato fundou a Clínica com uma ideia simples: tratamento
            odontológico não precisa ser assustador. Depois de mais de 3 mil pacientes atendidos, sua abordagem
            continua a mesma — ouvir antes de tratar, explicar cada etapa e devolver não só a saúde bucal, mas a
            confiança de sorrir sem pensar duas vezes.
          </p>

          <ul class="credentials">
            <li v-for="c in credentials" :key="c">
              <svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12l4 4 10-10" /></svg>
              {{ c }}
            </li>
          </ul>

          <div class="profile__footer">
            <p class="signature">Cássio Honorato</p>
            <a href="#contato" class="cta">
              Agendar uma avaliação
              <svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14M13 6l6 6-6 6" /></svg>
            </a>
          </div>
        </div>
      </div>

      <!-- Estatísticas -->
      <div class="stats">
        <div
          class="stat"
          v-for="(s, i) in stats"
          :key="s.label"
          :style="{ transitionDelay: i * 0.09 + 's' }"
        >
          <span class="stat__icon" v-html="s.icon"></span>
          <span class="stat__number">{{ s.prefix || '' }}{{ counters[s.label] }}{{ s.suffix || '' }}</span>
          <span class="stat__label">{{ s.label }}</span>
        </div>
      </div>

      <!-- Linha do tempo -->
      <div class="timeline">
        <p class="timeline__title">Uma trajetória construída ano após ano</p>
        <div class="timeline__track">
          <div class="timeline__line" aria-hidden="true"></div>
          <div
            v-for="(m, i) in milestones"
            :key="m.year"
            class="milestone"
            :class="i % 2 === 0 ? 'milestone--left' : 'milestone--right'"
            :style="{ transitionDelay: (i % 6) * 0.08 + 's' }"
          >
            <div class="milestone__dot"></div>
            <div class="milestone__card">
              <span class="milestone__year">{{ m.year }}</span>
              <p class="milestone__text">{{ m.text }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Galeria -->
      <div class="gallery">
        <p class="gallery__title">Um espaço pensado para o seu conforto</p>
        <div class="gallery__grid">
          <figure v-for="g in gallery" :key="g.caption" class="gallery__item">
            <img :src="g.img" :alt="g.caption" loading="lazy" />
            <figcaption>{{ g.caption }}</figcaption>
          </figure>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref, onMounted, onBeforeUnmount } from 'vue'

/* ---------- Dados fictícios — troque pelos reais ---------- */
const credentials = [
  'Graduada em Odontologia pela UFPI (2014)',
  'Especialização em Implantodontia — APCD (2017)',
  'Aperfeiçoamento em Harmonização Orofacial (2020)',
  'Membro da Associação Brasileira de Odontologia (ABO)'
]

const stats = [
  {
    label: 'anos de experiência',
    target: 9,
    suffix: '',
    icon: '<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 3"/></svg>'
  },
  {
    label: 'pacientes atendidos',
    target: 3200,
    suffix: '+',
    icon: '<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>'
  },
  {
    label: 'procedimentos realizados',
    target: 1800,
    suffix: '+',
    icon: '<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 21s7-6.4 7-12a7 7 0 1 0-14 0c0 5.6 7 12 7 12Z"/><path d="m9.5 10 1.8 1.8L15 8"/></svg>'
  },
  {
    label: 'avaliação média',
    target: 4.9,
    suffix: '',
    decimals: 1,
    icon: '<svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 3.5l2.6 5.4 5.9.8-4.3 4.1 1 5.9L12 16.9 6.8 19.7l1-5.9L3.5 9.7l5.9-.8L12 3.5z"/></svg>'
  }
]

const milestones = [
  { year: '2014', text: 'Formatura em Odontologia pela Universidade Federal do Piauí.' },
  { year: '2016', text: 'Primeiros passos como dentista associada, atendendo em clínica parceira.' },
  { year: '2017', text: 'Especialização em Implantodontia e abertura das portas da Clínica Sorriso.' },
  { year: '2020', text: 'Assume a direção clínica e amplia a equipe para atendimento multidisciplinar.' },
  { year: '2023', text: 'Certificação internacional em Harmonização Orofacial.' },
  { year: '2025', text: 'Mais de 3 mil sorrisos transformados e consultório totalmente reformado.' }
]

const gallery = [
  { img: 'https://picsum.photos/id/1076/800/1000', caption: 'Recepção' },
  { img: 'https://picsum.photos/id/106/800/1000', caption: 'Consultório 1' },
  { img: 'https://picsum.photos/id/48/800/1000', caption: 'Sala de esterilização' },
  { img: 'https://picsum.photos/id/119/800/1000', caption: 'Área de espera' }
]
/* ---------- Contadores animados ---------- */
const counters = reactive(
  Object.fromEntries(stats.map((s) => [s.label, s.decimals ? '0.0' : '0']))
)

function animateCounters() {
  const duration = 1400
  const start = performance.now()

  function tick(now) {
    const progress = Math.min((now - start) / duration, 1)
    const eased = 1 - Math.pow(1 - progress, 3)

    stats.forEach((s) => {
      const value = s.target * eased
      counters[s.label] = s.decimals ? value.toFixed(s.decimals) : Math.round(value).toLocaleString('pt-BR')
    })

    if (progress < 1) requestAnimationFrame(tick)
  }

  requestAnimationFrame(tick)
}

/* ---------- Reveal on scroll ---------- */
const sectionRef = ref(null)
const visible = ref(false)
let observer = null

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches

  if (prefersReducedMotion || !sectionRef.value) {
    visible.value = true
    stats.forEach((s) => {
      counters[s.label] = s.decimals
        ? s.target.toFixed(s.decimals)
        : s.target.toLocaleString('pt-BR')
    })
    return
  }

  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        visible.value = true
        animateCounters()
        observer.disconnect()
      }
    },
    { threshold: 0.15 }
  )
  observer.observe(sectionRef.value)
})
onBeforeUnmount(() => observer?.disconnect())
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.about {
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
  background: var(--paper);
  color: var(--ink);
  font-family: 'Manrope', sans-serif;
  padding: clamp(64px, 9vw, 120px) clamp(20px, 6vw, 64px);
}

.about__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet { position: absolute; border-radius: 50%; filter: blur(56px); mix-blend-mode: multiply; }
.facet--a { width: 30vw; height: 30vw; max-width: 420px; max-height: 420px; top: -8%; right: -10%; background: radial-gradient(circle, rgba(169, 131, 42, 0.16), transparent 70%); }
.facet--b { width: 24vw; height: 24vw; max-width: 340px; max-height: 340px; bottom: 10%; left: -8%; background: radial-gradient(circle, rgba(169, 131, 42, 0.12), transparent 70%); }

.about__inner { position: relative; z-index: 2; max-width: 1100px; margin: 0 auto; }

.about__head { text-align: center; max-width: 620px; margin: 0 auto clamp(44px, 6vw, 64px); }
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

/* ---------- Perfil ---------- */
.profile {
  display: grid;
  grid-template-columns: 0.75fr 1.25fr;
  gap: clamp(36px, 5vw, 72px);
  align-items: center;
  margin-bottom: clamp(56px, 7vw, 88px);
  opacity: 0; transform: translateY(18px);
  transition: opacity .7s ease, transform .7s ease;
}
.about.is-visible .profile { opacity: 1; transform: none; }

.profile__photo { position: relative; max-width: 340px; margin: 0 auto; }
.photo-frame {
  position: relative; z-index: 2;
  border-radius: 40% 60% 55% 45% / 55% 45% 60% 40%;
  overflow: hidden;
  aspect-ratio: 4 / 5;
  border: 6px solid #fff;
  box-shadow: 0 30px 60px -26px rgba(23, 18, 8, .38);
}
.photo-frame img { width: 100%; height: 100%; object-fit: cover; display: block; }

.photo-ring {
  position: absolute; inset: -14px; z-index: 1;
  border-radius: 42% 58% 53% 47% / 53% 47% 58% 42%;
  border: 2px dashed var(--gold-line);
  animation: ring-spin 40s linear infinite;
}
@keyframes ring-spin { to { transform: rotate(360deg); } }

.chip {
  position: absolute; z-index: 3;
  display: inline-flex; align-items: center; gap: 7px;
  background: #fff; border-radius: 999px; padding: 9px 14px;
  font-size: .74rem; font-weight: 700; color: var(--ink);
  box-shadow: 0 16px 30px -16px rgba(23, 18, 8, .4);
  white-space: nowrap;
}
.chip svg { color: var(--gold); flex-shrink: 0; }
.chip--top { top: -7%; right: -4%; }
.chip--bottom { bottom: 6%; left: -12%; }

@media (max-width: 900px) {
  .chip--top { top: -6%; right: 2%; }
}
@media (max-width: 420px) {
  .chip { font-size: .68rem; padding: 7px 11px; }
  .chip--top { top: -8%; right: 0; }
}

.profile__role { font-size: .78rem; letter-spacing: .1em; text-transform: uppercase; font-weight: 700; color: var(--gold-deep); margin: 0 0 6px; }
.profile__name { font-family: 'Fraunces', serif; font-weight: 600; font-size: clamp(1.6rem, 2.4vw, 2.1rem); margin: 0 0 6px; }
.profile__cro { font-size: .84rem; color: var(--ink-dim); margin: 0 0 18px; }
.profile__bio { font-size: .95rem; line-height: 1.75; color: var(--ink); margin: 0 0 20px; max-width: 56ch; }

.credentials { list-style: none; margin: 0 0 26px; padding: 0; display: flex; flex-direction: column; gap: 10px; }
.credentials li { display: flex; align-items: flex-start; gap: 9px; font-size: .86rem; color: var(--ink-dim); line-height: 1.5; }
.credentials svg { color: var(--gold); flex-shrink: 0; margin-top: 2px; }

.profile__footer { display: flex; align-items: center; justify-content: space-between; gap: 20px; flex-wrap: wrap; padding-top: 18px; border-top: 1px solid rgba(23, 18, 8, .08); }
.signature { font-family: 'Fraunces', serif; font-style: italic; font-weight: 500; font-size: 1.4rem; color: var(--gold-deep); margin: 0; position: relative; }
.signature::after { content: ''; position: absolute; left: 0; bottom: -6px; width: 70%; height: 2px; background: linear-gradient(90deg, var(--gold), transparent); border-radius: 2px; }

.cta {
  display: inline-flex; align-items: center; gap: 8px;
  padding: 12px 18px; border-radius: 12px; text-decoration: none;
  font-weight: 700; font-size: .86rem; color: #fff8ea;
  background: linear-gradient(120deg, var(--gold-deep), var(--gold) 55%, var(--gold-bright));
  box-shadow: 0 14px 28px -14px rgba(109, 79, 22, .55);
  transition: transform .2s ease, box-shadow .2s ease;
}
.cta:hover { transform: translateY(-1px); box-shadow: 0 18px 32px -14px rgba(109, 79, 22, .6); }

/* ---------- Estatísticas ---------- */
.stats {
  display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px;
  margin-bottom: clamp(56px, 7vw, 88px);
  padding: clamp(28px, 3.5vw, 38px) clamp(16px, 3vw, 30px);
  background: #fff; border-radius: 20px; border: 1px solid rgba(23, 18, 8, .08);
  box-shadow: 0 24px 48px -30px rgba(23, 18, 8, .3);
}
.stat {
  position: relative;
  text-align: center;
  padding: 10px 6px;
  border-radius: 14px;
  opacity: 0;
  transform: translateY(14px);
  transition: opacity .55s ease, transform .55s ease, background .25s ease;
  will-change: transform, opacity;
}
.about.is-visible .stat { opacity: 1; transform: translateY(0); }
.stat:hover { background: rgba(169, 131, 42, .06); transform: translateY(-3px); }
.stat:not(:last-child)::after {
  content: ''; position: absolute; top: 12%; bottom: 12%; right: -2px; width: 1px;
  background: rgba(23, 18, 8, .08);
}

.stat__icon {
  display: inline-flex; align-items: center; justify-content: center;
  width: 38px; height: 38px; border-radius: 50%; margin-bottom: 10px;
  background: rgba(169, 131, 42, .1); color: var(--gold-deep);
  transition: transform .3s ease, background .3s ease;
}
.stat:hover .stat__icon { background: var(--gold); color: #fff; transform: scale(1.08); }

.stat__number { display: block; font-family: 'Fraunces', serif; font-weight: 600; font-size: clamp(1.6rem, 2.6vw, 2.2rem); color: var(--gold-deep); font-variant-numeric: tabular-nums; }
.stat__label { display: block; font-size: .78rem; color: var(--ink-dim); margin-top: 4px; }

/* ---------- Timeline ---------- */
.timeline { margin-bottom: clamp(56px, 7vw, 88px); }
.timeline__title { text-align: center; font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.3rem; margin: 0 0 40px; }

.timeline__track { position: relative; display: flex; flex-direction: column; gap: 30px; }
.timeline__line {
  position: absolute; left: 50%; top: 0; bottom: 0; width: 2px;
  background: linear-gradient(180deg, transparent, var(--gold-line) 10%, var(--gold-line) 90%, transparent);
  transform: translateX(-50%);
}

.milestone {
  position: relative; width: 50%;
  opacity: 0; transform: translateY(16px);
  transition: opacity .6s ease, transform .6s ease;
}
.about.is-visible .milestone { opacity: 1; transform: none; }
.milestone--left { left: 0; padding-right: 44px; text-align: right; }
.milestone--right { left: 50%; padding-left: 44px; text-align: left; }

.milestone__dot {
  position: absolute; top: 6px; width: 14px; height: 14px; border-radius: 50%;
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  box-shadow: 0 0 0 5px #faf9f5, 0 0 0 6px var(--gold-line);
}
.milestone--left .milestone__dot { right: -7px; }
.milestone--right .milestone__dot { left: -7px; }

.milestone__card {
  display: inline-block; background: #fff; border: 1px solid rgba(23, 18, 8, .08);
  border-radius: 14px; padding: 14px 18px; box-shadow: 0 18px 36px -26px rgba(23, 18, 8, .3);
}
.milestone__year { display: inline-block; font-family: 'Fraunces', serif; font-weight: 600; color: var(--gold-deep); font-size: .95rem; margin-bottom: 4px; }
.milestone__text { margin: 0; font-size: .84rem; line-height: 1.55; color: var(--ink-dim); }

/* ---------- Galeria ---------- */
.gallery__title { text-align: center; font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.3rem; margin: 0 0 26px; }
.gallery__grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
.gallery__item {
  margin: 0; border-radius: 16px; overflow: hidden; position: relative;
  aspect-ratio: 3 / 4; box-shadow: 0 20px 40px -28px rgba(23, 18, 8, .32);
}
.gallery__item img { width: 100%; height: 100%; object-fit: cover; display: block; transition: transform .5s ease; }
.gallery__item:hover img { transform: scale(1.07); }
.gallery__item figcaption {
  position: absolute; left: 0; right: 0; bottom: 0;
  background: linear-gradient(0deg, rgba(23,18,8,.72), transparent);
  color: #fbf3de; font-size: .76rem; font-weight: 700; padding: 22px 12px 10px;
}

@media (max-width: 900px) {
  .profile { grid-template-columns: 1fr; }
  .profile__photo { max-width: 260px; }
  .chip--top { right: 2%; }
  .chip--bottom { left: 2%; }
  .stats { grid-template-columns: repeat(2, 1fr); row-gap: 22px; }
  .stat::after { display: none; }
  .stat:nth-child(1)::before,
  .stat:nth-child(2)::before {
    content: ''; position: absolute; left: 12%; right: 12%; bottom: -11px; height: 1px;
    background: rgba(23, 18, 8, .08);
  }
  .gallery__grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 720px) {
  .timeline__line { left: 20px; }
  .milestone, .milestone--left, .milestone--right {
    width: 100%; left: 0; padding-left: 46px; padding-right: 0; text-align: left;
  }
  .milestone--left .milestone__dot, .milestone--right .milestone__dot { left: 13px; right: auto; }
}

@media (prefers-reduced-motion: reduce) {
  .profile, .milestone, .stat, .stat__icon { opacity: 1 !important; transform: none !important; transition: none !important; }
  .photo-ring { animation: none; }
  .gallery__item img { transition: none; }
}
</style>