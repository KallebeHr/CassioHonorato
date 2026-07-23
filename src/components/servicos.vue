<template>
  <section class="services" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="services__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="services__inner">
      <div class="services__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Nossos serviços</p>
        </div>
        <h2 class="headline">Tratamentos <span class="word--gold">lapidados</span> para cada sorriso</h2>
        <p class="subtext">
          Da prevenção à estética avançada — cada procedimento conduzido com o mesmo
          olhar de precisão do consultório.
        </p>
      </div>

      <!-- Tabs editoriais: sublinhado simples, sem medir DOM, sem JS de posição -->
      <div class="tabs" role="tablist" aria-label="Filtrar serviços por categoria">
        <button
          v-for="cat in categories"
          :key="cat.id"
          type="button"
          role="tab"
          class="tab"
          :class="{ 'is-active': activeCategory === cat.id }"
          :aria-selected="activeCategory === cat.id"
          @click="activeCategory = cat.id"
        >
          {{ cat.label }}
        </button>
      </div>

      <!--
        v-show mantido: todos os cards existem no DOM o tempo todo,
        trocar categoria é só display toggle — sem recriar elementos,
        sem recalcular layout, sem travada.
      -->
      <div class="grid">
        <article
          v-for="(s, i) in services"
          :key="s.id"
          v-show="activeCategory === 'todos' || s.category === activeCategory"
          class="card"
          :style="{ transitionDelay: (i % 3) * 0.07 + 's' }"
        >
          <span class="card__index">{{ String(i + 1).padStart(2, '0') }}</span>

          <div class="card__icon">
            <svg viewBox="0 0 24 24" fill="none" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" v-html="s.icon"></svg>
          </div>

          <p class="card__tag">{{ categoryLabel(s.category) }}</p>
          <h3 class="card__title">{{ s.title }}</h3>
          <p class="card__desc">{{ s.desc }}</p>

          <ul class="card__stats">
            <li>
              <span class="stat__label">Duração</span>
              <span class="stat__value">{{ s.duration }}</span>
            </li>
            <li>
              <span class="stat__label">Sessões</span>
              <span class="stat__value">{{ s.sessions }}</span>
            </li>
            <li>
              <span class="stat__label">Indicado para</span>
              <span class="stat__value">{{ s.indicated }}</span>
            </li>
          </ul>

          <a href="#agendar" class="card__cta">Agendar avaliação →</a>
        </article>

        <p v-if="!hasVisibleCard" class="grid__empty">
          Nenhum serviço encontrado nesta categoria.
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

/**
 * Sem GSAP, sem TransitionGroup, sem tilt 3D — só CSS.
 * A única animação é a entrada da seção ao rolar até ela (IntersectionObserver)
 * e um hover leve via transform/box-shadow (GPU, sem custo de layout).
 * Ajuste os itens de `services` com os procedimentos reais da clínica.
 */

const sectionRef = ref(null)
const visible = ref(false)

const categories = [
  { id: 'todos', label: 'Todos' },
  { id: 'estetica', label: 'Estética' },
  { id: 'preventiva', label: 'Preventiva' },
  { id: 'ortodontia', label: 'Ortodontia' },
  { id: 'cirurgica', label: 'Cirurgia' }
]

function categoryLabel(id) {
  return categories.find((c) => c.id === id)?.label ?? ''
}

const services = [
  {
    id: 'clareamento',
    category: 'estetica',
    title: 'Clareamento Dental',
    desc: 'Clareamento a laser ou com moldeira, com resultado uniforme e seguro para o esmalte.',
    duration: '60 min por sessão',
    sessions: '1 a 2 sessões',
    indicated: 'Dentes escurecidos por idade, café ou tabaco',
    icon: '<path d="M12 3l1.8 5.2L19 10l-5.2 1.8L12 17l-1.8-5.2L5 10l5.2-1.8L12 3Z"/>'
  },
  {
    id: 'lentes',
    category: 'estetica',
    title: 'Lentes em Resina',
    desc: 'Camada fina aplicada diretamente sobre o dente para corrigir forma, cor e alinhamento aparente.',
    duration: '90 a 120 min',
    sessions: '1 sessão por arco',
    indicated: 'Pequenas imperfeições estéticas e diastemas',
    icon: '<path d="M6 4h12l1 5-7 11L5 9Z"/><path d="M9 4l3 5 3-5M6 9h12"/>'
  },
  {
    id: 'facetas',
    category: 'estetica',
    title: 'Facetas de Porcelana',
    desc: 'Peças cerâmicas sob medida, feitas em laboratório para máxima naturalidade e durabilidade.',
    duration: '2 consultas de planejamento',
    sessions: '2 a 3 sessões',
    indicated: 'Transformações estéticas mais amplas',
    icon: '<path d="M12 3l7 4-7 4-7-4 7-4Z"/><path d="M5 11l7 4 7-4M5 15l7 4 7-4"/>'
  },
  {
    id: 'limpeza',
    category: 'preventiva',
    title: 'Limpeza e Profilaxia',
    desc: 'Remoção de placa e tártaro com polimento, prevenindo cáries e doenças gengivais.',
    duration: '40 min',
    sessions: 'Recomendado a cada 6 meses',
    indicated: 'Manutenção de rotina para todas as idades',
    icon: '<path d="M12 3l7 3v6c0 4.5-3 7.5-7 9-4-1.5-7-4.5-7-9V6l7-3Z"/><path d="M9 12l2 2 4-4"/>'
  },
  {
    id: 'avaliacao',
    category: 'preventiva',
    title: 'Avaliação e Diagnóstico',
    desc: 'Exame clínico completo com plano de tratamento personalizado e transparente.',
    duration: '30 min',
    sessions: 'Sessão única',
    indicated: 'Primeira consulta ou check-up periódico',
    icon: '<circle cx="10.5" cy="10.5" r="6"/><path d="M15 15l5 5"/>'
  },
  {
    id: 'aparelho',
    category: 'ortodontia',
    title: 'Aparelho Ortodôntico',
    desc: 'Correção do alinhamento dentário com acompanhamento mensal até o resultado final.',
    duration: 'Consultas de 30 min',
    sessions: 'Tratamento de 12 a 24 meses',
    indicated: 'Dentes desalinhados ou mordida incorreta',
    icon: '<path d="M4 12h16M4 12a4 4 0 0 1 4-4h1M4 12a4 4 0 0 0 4 4h1M20 12a4 4 0 0 0-4-4h-1M20 12a4 4 0 0 1-4 4h-1"/><circle cx="9" cy="8" r="1.3" fill="currentColor" stroke="none"/><circle cx="15" cy="16" r="1.3" fill="currentColor" stroke="none"/>'
  },
  {
    id: 'alinhadores',
    category: 'ortodontia',
    title: 'Alinhadores Invisíveis',
    desc: 'Placas transparentes e removíveis, trocadas periodicamente ao longo do tratamento.',
    duration: 'Trocas a cada 7 a 10 dias',
    sessions: 'Retornos a cada 6 a 8 semanas',
    indicated: 'Quem busca discrição durante o tratamento',
    icon: '<path d="M4 9c0-2 3.5-3 8-3s8 1 8 3-3.5 3-8 3-8-1-8-3Z"/><path d="M4 15c0-2 3.5-3 8-3s8 1 8 3-3.5 3-8 3-8-1-8-3Z"/>'
  },
  {
    id: 'cirurgia',
    category: 'cirurgica',
    title: 'Cirurgia Oral Menor',
    desc: 'Extrações e pequenos procedimentos cirúrgicos realizados com técnica e conforto.',
    duration: '45 a 60 min',
    sessions: 'Conforme indicação clínica',
    indicated: 'Dentes inclusos, sisos e extrações complexas',
    icon: '<path d="M7 3c3 0 4 2 5 2s2-2 5-2c2 0 3 2 3 5 0 6-3 8-3 12 0 2-1 3-2 3s-2-2-2-4-1-3-1-3-1 1-1 3-1 4-2 4-2-1-2-3c0-4-3-6-3-12 0-3 1-5 3-5Z"/>'
  },
  {
    id: 'implantes',
    category: 'cirurgica',
    title: 'Implantes Dentários',
    desc: 'Substituição de dentes perdidos com raiz artificial de titânio e coroa personalizada.',
    duration: 'Cirurgia de 60 a 90 min',
    sessions: 'Processo em etapas, 3 a 6 meses',
    indicated: 'Perda de um ou mais dentes',
    icon: '<path d="M12 3v6"/><path d="M8 9h8l-1.5 3h-5L8 9Z"/><path d="M9.5 12l.5 7 2 2 2-2 .5-7"/><path d="M9.8 14.5h4.4M9.8 17h4.4"/>'
  }
]

const activeCategory = ref('todos')

const hasVisibleCard = computed(() =>
  activeCategory.value === 'todos' || services.some((s) => s.category === activeCategory.value)
)

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
    { threshold: 0.12 }
  )
  observer.observe(sectionRef.value)
})

onBeforeUnmount(() => {
  observer?.disconnect()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.services {
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

.services__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 30vw; height: 30vw; max-width: 420px; max-height: 420px;
  top: -8%; right: -10%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.18), transparent 70%);
  filter: blur(50px);
  mix-blend-mode: multiply;
}

.services__inner { position: relative; z-index: 2; max-width: 1180px; margin: 0 auto; }

.services__head,
.tabs,
.card {
  opacity: 0;
  transform: translateY(18px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.services.is-visible .services__head { opacity: 1; transform: none; }
.services.is-visible .tabs { opacity: 1; transform: none; transition-delay: 0.08s; }
.services.is-visible .card { opacity: 1; transform: none; }

.services__head { text-align: center; max-width: 620px; margin: 0 auto clamp(36px, 5vw, 52px); }
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

/* Tabs editoriais: uma linha fina de base, sublinhado dourado só no ativo. Sem JS de posição. */
.tabs {
  display: flex; flex-wrap: wrap; justify-content: center; gap: clamp(20px, 3vw, 36px);
  margin: 0 auto clamp(40px, 6vw, 60px);
  padding-bottom: 14px;
  border-bottom: 1px solid rgba(23, 18, 8, .1);
  max-width: 760px;
}
.tab {
  position: relative;
  background: none; border: none; cursor: pointer;
  padding: 4px 2px 14px;
  font-family: 'Fraunces', serif; font-style: italic; font-weight: 500;
  font-size: .95rem; color: var(--ink-dim);
  transition: color .25s ease;
}
.tab::after {
  content: ''; position: absolute; left: 0; right: 0; bottom: -1px; height: 2px;
  background: linear-gradient(90deg, var(--gold-soft), var(--gold));
  transform: scaleX(0); transform-origin: center;
  transition: transform .3s ease;
}
.tab:hover { color: var(--ink); }
.tab.is-active { color: var(--gold-deep); font-weight: 600; }
.tab.is-active::after { transform: scaleX(1); }

.grid {
  display: grid; grid-template-columns: repeat(3, 1fr); gap: clamp(18px, 2.4vw, 28px);
}
.grid__empty { grid-column: 1 / -1; text-align: center; color: var(--ink-dim); padding: 40px 0; }

/* Card sem clip-path: friso dourado no topo em vez de corte de canto, elimina o bug
   de sombra "vazando" na área cortada e fica mais discreto/premium. */
.card {
  position: relative;
  display: flex; flex-direction: column;
  background: linear-gradient(160deg, #ffffff, var(--paper) 130%);
  border: 1px solid rgba(23, 18, 8, .08);
  border-radius: 14px;
  padding: clamp(26px, 2.6vw, 32px) clamp(22px, 2.4vw, 26px) clamp(24px, 2.2vw, 28px);
  box-shadow: 0 16px 34px -22px rgba(23, 18, 8, .28);
  transition: opacity 0.6s ease, transform 0.6s ease, box-shadow .3s ease;
}
.card::before {
  content: ''; position: absolute; top: 0; left: 24px; right: 24px; height: 3px;
  background: linear-gradient(90deg, transparent, var(--gold-soft), var(--gold), var(--gold-soft), transparent);
  border-radius: 0 0 3px 3px;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 40px -20px rgba(23, 18, 8, .32);
}

.card__index {
  position: absolute; top: 22px; right: 24px;
  font-family: 'Fraunces', serif; font-style: italic; font-size: .78rem;
  color: var(--gold-line); letter-spacing: .04em;
}

.card__icon {
  display: inline-flex; align-items: center; justify-content: center;
  width: 46px; height: 46px; border-radius: 50%;
  border: 1px solid var(--gold-line); background: rgba(169,131,42,.06);
  margin-bottom: 18px;
}
.card__icon svg { width: 21px; height: 21px; stroke: var(--gold-deep); }

.card__tag {
  font-family: 'Fraunces', serif; font-style: italic; font-weight: 500;
  font-size: .78rem; letter-spacing: .03em; color: var(--gold);
  margin: 0 0 4px;
}
.card__title { font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.18rem; margin: 0 0 8px; color: var(--ink); }
.card__desc { font-size: .88rem; line-height: 1.55; color: var(--ink-dim); margin: 0 0 18px; }

.card__stats {
  list-style: none; margin: 0 0 18px; padding: 16px 0 0;
  border-top: 1px solid rgba(23,18,8,.08);
  display: flex; flex-direction: column; gap: 9px;
}
.card__stats li { display: flex; justify-content: space-between; gap: 12px; font-size: .8rem; }
.stat__label { color: var(--ink-dim); }
.stat__value { color: var(--ink); font-weight: 700; text-align: right; }

.card__cta {
  margin-top: auto;
  display: inline-flex; font-size: .82rem; font-weight: 700;
  color: var(--gold-deep); text-decoration: none; border-bottom: 1px solid var(--gold-line);
  padding-bottom: 2px; width: fit-content;
  transition: border-color .25s ease, color .25s ease;
}
.card__cta:hover { border-color: var(--gold); color: var(--gold); }

@media (max-width: 980px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 640px) {
  .grid { grid-template-columns: 1fr; }
  .tabs { gap: 16px; }
}

@media (prefers-reduced-motion: reduce) {
  .services__head, .tabs, .card {
    opacity: 1 !important; transform: none !important; transition: none !important;
  }
  .card:hover { transform: none; }
}
</style>