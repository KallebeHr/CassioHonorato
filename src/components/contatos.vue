<template>
  <section class="contact" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="contact__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="contact__inner">
      <div class="contact__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Fale com a gente</p>
        </div>
        <h2 class="headline">Vamos <span class="word--gold">conversar</span> sobre o seu sorriso</h2>
        <p class="subtext">
          Preencha o formulário e envie direto pelo WhatsApp — ou fale com a gente pelos canais abaixo.
        </p>
      </div>

      <div class="contact__grid">
        <!-- Formulário -->
        <form class="form" novalidate @submit="handleSubmit">
          <span class="form__badge">
            <span class="form__badge-dot"></span> Resposta em até 1h úteis
          </span>

          <div class="field" :class="{ 'is-invalid': touched.nome && errors.nome }">
            <label for="c-nome">Nome</label>
            <input
              id="c-nome"
              v-model="form.nome"
              type="text"
              placeholder="Como podemos te chamar?"
              autocomplete="name"
              @blur="touched.nome = true"
            />
            <span class="field__icon">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M20 21a8 8 0 1 0-16 0" /><circle cx="12" cy="7" r="4" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.nome && errors.nome">{{ errors.nome }}</p>
          </div>

          <div class="field" :class="{ 'is-invalid': touched.telefone && errors.telefone }">
            <label for="c-tel">WhatsApp</label>
            <input
              id="c-tel"
              v-model="form.telefone"
              type="tel"
              inputmode="numeric"
              placeholder="(00) 00000-0000"
              autocomplete="tel"
              @input="onPhoneInput"
              @blur="touched.telefone = true"
            />
            <span class="field__icon">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.telefone && errors.telefone">{{ errors.telefone }}</p>
          </div>

          <div class="field field--select">
            <label>Tipo de atendimento</label>
            <div class="segmented" role="tablist">
              <button
                v-for="t in tiposAtendimento"
                :key="t.id"
                type="button"
                class="segmented__tab"
                :class="{ 'is-active': form.tipo === t.id }"
                @click="form.tipo = t.id"
              >
                <span v-html="t.icon" class="segmented__icon"></span>
                {{ t.label }}
              </button>
            </div>
          </div>

          <div class="field">
            <label for="c-msg">Mensagem <span class="field__optional">(opcional)</span></label>
            <textarea
              id="c-msg"
              v-model="form.mensagem"
              rows="3"
              placeholder="Conte rapidamente o que você precisa..."
            ></textarea>
            <span class="field__icon field__icon--textarea">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
              </svg>
            </span>
          </div>

          <button type="submit" class="submit" :class="status" :disabled="status === 'sending'">
            <span v-if="status === 'idle'" class="submit__content">
              Enviar pelo WhatsApp
              <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z" /></svg>
            </span>
            <span v-else-if="status === 'sending'" class="submit__content">
              <svg class="spin" viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M12 2a10 10 0 0 1 10 10" /></svg>
              Preparando mensagem...
            </span>
            <span v-else class="submit__content">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12l4 4 10-10" /></svg>
              Aberto no WhatsApp!
            </span>
          </button>

          <p class="form__fallback">
            Prefere e-mail? <a :href="mailtoLink">Escreva para {{ clinic.email }}</a>
          </p>
        </form>

        <!-- Painel lateral -->
        <aside class="side">
          <div class="status-card" :class="{ 'is-open': isOpenNow }">
            <span class="status-card__dot"></span>
            <div>
              <p class="status-card__state">{{ isOpenNow ? 'Estamos atendendo agora' : 'Fora do horário de atendimento' }}</p>
              <p class="status-card__next">{{ nextChangeLabel }}</p>
            </div>
          </div>

          <ul class="channels">
            <li v-for="ch in channels" :key="ch.id" class="channel">
              <span class="channel__icon" :style="{ background: ch.bg, color: ch.fg }" v-html="ch.icon"></span>
              <div class="channel__text">
                <span class="channel__label">{{ ch.label }}</span>
                <span class="channel__value">{{ ch.value }}</span>
              </div>
              <button
                type="button"
                class="channel__action"
                :title="ch.action === 'copy' ? 'Copiar' : 'Abrir'"
                @click="ch.action === 'copy' ? copy(ch.raw, ch.id) : go(ch.href)"
              >
                <svg v-if="copiedId === ch.id" viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12l4 4 10-10" /></svg>
                <svg v-else-if="ch.action === 'copy'" viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="11" height="11" rx="2" /><path d="M5 15V5a2 2 0 0 1 2-2h10" /></svg>
                <svg v-else viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M7 17L17 7M7 7h10v10" /></svg>
              </button>
            </li>
          </ul>

          <div class="hours-card">
            <p class="hours-card__title">Horário de atendimento</p>
            <ul class="hours-card__list">
              <li v-for="(h, i) in schedule" :key="h.day" :class="{ 'is-today': i === todayIndex }">
                <span>{{ h.day }}</span>
                <strong>{{ h.open === null ? 'Fechado' : `${h.open} às ${h.close}` }}</strong>
              </li>
            </ul>
          </div>

          <div class="social">
            <a
              v-for="s in socials"
              :key="s.id"
              :href="s.href"
              target="_blank"
              rel="noopener"
              class="social__link"
              :aria-label="s.label"
              v-html="s.icon"
            ></a>
          </div>
        </aside>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref, computed, onMounted, onBeforeUnmount } from 'vue'

/* ---------- Dados — troque pelos seus ---------- */
const clinic = reactive({
  whatsappRaw: '5586981475263',
  phoneDisplay: '(86) 98147-5263',
  email: 'contato@cassiohonorato.com.br',
  instagram: '@cassio.honorato_',
  instagramHref: 'https://instagram.com/cassio.honorato_'
})

/* domingo=0 ... sábado=6. open/close em "HH:MM", null = fechado */
const schedule = [
  { day: 'Domingo', open: null, close: null },
  { day: 'Segunda', open: '08:00', close: '19:00' },
  { day: 'Terça', open: '08:00', close: '19:00' },
  { day: 'Quarta', open: '08:00', close: '19:00' },
  { day: 'Quinta', open: '08:00', close: '19:00' },
  { day: 'Sexta', open: '08:00', close: '19:00' },
  { day: 'Sábado', open: '08:00', close: '13:00' }
]

const tiposAtendimento = [
  { id: 'avaliacao', label: 'Avaliação', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 3"/></svg>' },
  { id: 'emergencia', label: 'Emergência', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 9v4M12 17h.01"/><path d="M10.3 3.86 1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L14.7 3.86a2 2 0 0 0-3.4 0Z"/></svg>' },
  { id: 'duvida', label: 'Dúvida geral', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9.1 9a3 3 0 1 1 5.82 1c-.4 1.2-2.02 1.6-2.62 2.66-.2.36-.3.79-.3 1.34"/><path d="M12 18h.01"/><circle cx="12" cy="12" r="9"/></svg>' }
]

const channels = [
  {
    id: 'whats',
    label: 'WhatsApp',
    value: clinic.phoneDisplay,
    raw: clinic.phoneDisplay,
    action: 'open',
    href: `https://wa.me/${clinic.whatsappRaw}`,
    bg: 'rgba(37, 211, 102, .12)', fg: '#1f9c52',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z"/></svg>'
  },
  {
    id: 'tel',
    label: 'Telefone',
    value: clinic.phoneDisplay,
    raw: clinic.phoneDisplay,
    action: 'copy',
    bg: 'rgba(169, 131, 42, .12)', fg: '#6d4f16',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"/></svg>'
  },
  {
    id: 'mail',
    label: 'E-mail',
    value: clinic.email,
    raw: clinic.email,
    action: 'copy',
    bg: 'rgba(47, 111, 237, .12)', fg: '#2f6fed',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 6-10 7L2 6"/></svg>'
  },
  {
    id: 'insta',
    label: 'Instagram',
    value: clinic.instagram,
    raw: clinic.instagram,
    action: 'open',
    href: clinic.instagramHref,
    bg: 'rgba(214, 60, 122, .12)', fg: '#d63c7a',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1"/></svg>'
  }
]

const socials = [
  { id: 'insta', label: 'Instagram', href: clinic.instagramHref, icon: channels.find(c => c.id === 'insta').icon },
  { id: 'whats', label: 'WhatsApp', href: `https://wa.me/${clinic.whatsappRaw}`, icon: channels.find(c => c.id === 'whats').icon }
]

const mailtoLink = computed(() => {
  const subject = encodeURIComponent('Contato pelo site')
  const body = encodeURIComponent(`Olá! Meu nome é ${form.nome || '___'}.\n\n${form.mensagem || ''}`)
  return `mailto:${clinic.email}?subject=${subject}&body=${body}`
})

/* ---------- Status ao vivo ---------- */
const now = ref(new Date())
let clock = null
onMounted(() => { clock = setInterval(() => (now.value = new Date()), 30000) })
onBeforeUnmount(() => clearInterval(clock))

const todayIndex = computed(() => now.value.getDay())

function toMinutes(hhmm) {
  const [h, m] = hhmm.split(':').map(Number)
  return h * 60 + m
}

const isOpenNow = computed(() => {
  const today = schedule[todayIndex.value]
  if (!today.open) return false
  const mins = now.value.getHours() * 60 + now.value.getMinutes()
  return mins >= toMinutes(today.open) && mins < toMinutes(today.close)
})

const nextChangeLabel = computed(() => {
  const today = schedule[todayIndex.value]
  if (isOpenNow.value) return `Fecha às ${today.close}`

  for (let i = 0; i < 8; i++) {
    const idx = (todayIndex.value + i) % 7
    const day = schedule[idx]
    if (day.open) {
      if (i === 0) return `Abre hoje às ${day.open}`
      if (i === 1) return `Abre amanhã às ${day.open}`
      return `Abre ${day.day.toLowerCase()} às ${day.open}`
    }
  }
  return ''
})

/* ---------- Formulário ---------- */
const form = reactive({ nome: '', telefone: '', tipo: 'avaliacao', mensagem: '' })
const touched = reactive({ nome: false, telefone: false })
const status = ref('idle') // idle | sending | success

const errors = computed(() => ({
  nome: form.nome.trim().length < 2 ? 'Conta seu nome pra gente te chamar certinho.' : '',
  telefone: form.telefone.replace(/\D/g, '').length < 10 ? 'Confere esse número, parece incompleto.' : ''
}))
const isValid = computed(() => !errors.value.nome && !errors.value.telefone)

function onPhoneInput() {
  const digits = form.telefone.replace(/\D/g, '').slice(0, 11)
  let out = digits
  if (digits.length > 2) out = `(${digits.slice(0, 2)}) ${digits.slice(2)}`
  if (digits.length > 7) {
    const split = digits.length > 10 ? 7 : 6
    out = `(${digits.slice(0, 2)}) ${digits.slice(2, split)}-${digits.slice(split)}`
  }
  form.telefone = out
}

function handleSubmit(e) {
  e.preventDefault()
  touched.nome = true
  touched.telefone = true
  if (!isValid.value) return

  status.value = 'sending'

  const tipoLabel = tiposAtendimento.find((t) => t.id === form.tipo)?.label
  const lines = [
    `Olá! Meu nome é ${form.nome}.`,
    `Tipo de atendimento: ${tipoLabel}`,
    `Meu WhatsApp: ${form.telefone}`
  ]
  if (form.mensagem.trim()) lines.push('', form.mensagem.trim())

  setTimeout(() => {
    window.open(`https://wa.me/${clinic.whatsappRaw}?text=${encodeURIComponent(lines.join('\n'))}`, '_blank', 'noopener')
    status.value = 'success'
    setTimeout(() => {
      status.value = 'idle'
      form.nome = ''
      form.telefone = ''
      form.mensagem = ''
      form.tipo = 'avaliacao'
      touched.nome = false
      touched.telefone = false
    }, 3200)
  }, 700)
}

/* ---------- Ações dos cards ---------- */
const copiedId = ref('')
async function copy(text, id) {
  try {
    await navigator.clipboard.writeText(text)
  } catch {
    const ta = document.createElement('textarea')
    ta.value = text
    document.body.appendChild(ta)
    ta.select()
    document.execCommand('copy')
    document.body.removeChild(ta)
  }
  copiedId.value = id
  setTimeout(() => (copiedId.value = ''), 1800)
}
function go(href) { window.open(href, '_blank', 'noopener') }

/* ---------- Reveal on scroll ---------- */
const sectionRef = ref(null)
const visible = ref(false)
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
onBeforeUnmount(() => observer?.disconnect())
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.contact {
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

.contact__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 32vw; height: 32vw; max-width: 460px; max-height: 460px;
  top: -12%; right: -10%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.15), transparent 70%);
  filter: blur(54px);
  mix-blend-mode: multiply;
}

.contact__inner { position: relative; z-index: 2; max-width: 1080px; margin: 0 auto; }

.contact__head { text-align: center; max-width: 620px; margin: 0 auto clamp(36px, 5vw, 52px); }
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

.contact__grid {
  display: grid;
  grid-template-columns: 1.15fr 0.85fr;
  gap: clamp(24px, 3.5vw, 48px);
  align-items: start;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.contact.is-visible .contact__grid { opacity: 1; transform: none; }

/* ---------- Formulário ---------- */
.form {
  position: relative;
  background: #fff;
  border: 1px solid rgba(23, 18, 8, .08);
  border-radius: 20px;
  padding: clamp(26px, 3vw, 36px);
  box-shadow: 0 24px 48px -28px rgba(23, 18, 8, .3);
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.form__badge {
  align-self: flex-start;
  display: inline-flex; align-items: center; gap: 7px;
  font-size: .72rem; font-weight: 700; letter-spacing: .03em;
  color: var(--gold-deep); background: rgba(169, 131, 42, .1);
  padding: 6px 12px; border-radius: 999px; margin-bottom: 2px;
}
.form__badge-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--gold); }

.field { position: relative; display: flex; flex-direction: column; gap: 6px; }
.field label { font-size: .78rem; font-weight: 700; color: var(--ink-dim); }
.field__optional { font-weight: 500; color: rgba(23,18,8,.4); }

.field input,
.field textarea {
  font-family: 'Manrope', sans-serif;
  font-size: .92rem;
  padding: 12px 14px 12px 40px;
  border-radius: 12px;
  border: 1px solid rgba(23, 18, 8, .14);
  background: #fbfaf6;
  color: var(--ink);
  outline: none;
  transition: border-color .2s ease, box-shadow .2s ease, background .2s ease;
  resize: vertical;
}
.field textarea { padding-left: 40px; min-height: 84px; }
.field input:focus,
.field textarea:focus {
  border-color: var(--gold);
  background: #fff;
  box-shadow: 0 0 0 3px rgba(169, 131, 42, .14);
}
.field.is-invalid input { border-color: #c94b4b; box-shadow: 0 0 0 3px rgba(201, 75, 75, .1); }

.field__icon {
  position: absolute; left: 13px; top: 33px;
  color: rgba(23, 18, 8, .35); pointer-events: none;
  display: flex;
}
.field__icon--textarea { top: 33px; }

.field__error { margin: 0; font-size: .74rem; color: #c94b4b; font-weight: 600; }

.segmented { display: flex; gap: 8px; flex-wrap: wrap; }
.segmented__tab {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 13px; border-radius: 999px; border: 1px solid rgba(23, 18, 8, .12);
  background: none; color: var(--ink-dim); font-size: .8rem; font-weight: 600;
  cursor: pointer; transition: all .2s ease; font-family: 'Manrope', sans-serif;
}
.segmented__tab.is-active {
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  border-color: transparent; color: #241a08;
}
.segmented__icon { display: inline-flex; }

.submit {
  margin-top: 4px;
  display: inline-flex; align-items: center; justify-content: center;
  padding: 14px 20px; border-radius: 12px; border: none; cursor: pointer;
  font-family: 'Manrope', sans-serif; font-weight: 700; font-size: .94rem;
  color: #fff8ea;
  background: linear-gradient(120deg, var(--gold-deep), var(--gold) 55%, var(--gold-bright));
  box-shadow: 0 14px 28px -14px rgba(109, 79, 22, .55);
  transition: transform .2s ease, box-shadow .2s ease, background .3s ease;
}
.submit:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 18px 32px -14px rgba(109, 79, 22, .6); }
.submit:disabled { cursor: default; }
.submit.success { background: linear-gradient(120deg, #1f9c52, #25d366); box-shadow: 0 14px 28px -14px rgba(31, 156, 82, .5); }
.submit__content { display: inline-flex; align-items: center; gap: 9px; }
.submit .spin { animation: spin 1s linear infinite; }
@keyframes spin { to { transform: rotate(360deg); } }

.form__fallback { margin: 0; font-size: .8rem; color: var(--ink-dim); text-align: center; }
.form__fallback a { color: var(--gold-deep); font-weight: 700; text-decoration: none; }
.form__fallback a:hover { text-decoration: underline; }

/* ---------- Lateral ---------- */
.side { display: flex; flex-direction: column; gap: 18px; }

.status-card {
  display: flex; align-items: center; gap: 12px;
  background: #fff; border: 1px solid rgba(23, 18, 8, .08); border-radius: 16px;
  padding: 16px 18px;
  box-shadow: 0 20px 40px -28px rgba(23, 18, 8, .28);
}
.status-card__dot {
  width: 11px; height: 11px; border-radius: 50%; flex-shrink: 0;
  background: #b9b3a0;
}
.status-card.is-open .status-card__dot {
  background: #25d366;
  box-shadow: 0 0 0 0 rgba(37, 211, 102, .5);
  animation: dot-pulse 1.8s ease-out infinite;
}
@keyframes dot-pulse {
  0% { box-shadow: 0 0 0 0 rgba(37, 211, 102, .45); }
  100% { box-shadow: 0 0 0 9px rgba(37, 211, 102, 0); }
}
.status-card__state { margin: 0; font-weight: 700; font-size: .88rem; }
.status-card__next { margin: 2px 0 0; font-size: .78rem; color: var(--ink-dim); }

.channels { list-style: none; margin: 0; padding: 0; display: flex; flex-direction: column; gap: 10px; }
.channel {
  display: flex; align-items: center; gap: 12px;
  background: #fff; border: 1px solid rgba(23, 18, 8, .08); border-radius: 14px;
  padding: 12px 14px;
  box-shadow: 0 16px 32px -26px rgba(23, 18, 8, .28);
  transition: transform .2s ease;
}
.channel:hover { transform: translateY(-1px); }
.channel__icon {
  width: 38px; height: 38px; border-radius: 10px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
}
.channel__text { display: flex; flex-direction: column; min-width: 0; flex: 1; }
.channel__label { font-size: .7rem; font-weight: 700; text-transform: uppercase; letter-spacing: .04em; color: var(--ink-dim); }
.channel__value { font-size: .88rem; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.channel__action {
  width: 32px; height: 32px; border-radius: 9px; flex-shrink: 0;
  border: 1px solid rgba(23, 18, 8, .1); background: none; color: var(--ink-dim);
  display: flex; align-items: center; justify-content: center; cursor: pointer;
  transition: background .2s ease, color .2s ease, border-color .2s ease;
}
.channel__action:hover { background: var(--gold); color: #fff; border-color: var(--gold); }

.hours-card {
  background: #fff; border: 1px solid rgba(23, 18, 8, .08); border-radius: 16px;
  padding: 18px 20px;
  box-shadow: 0 20px 40px -28px rgba(23, 18, 8, .28);
}
.hours-card__title { margin: 0 0 12px; font-size: .78rem; font-weight: 700; color: var(--ink-dim); }
.hours-card__list { list-style: none; margin: 0; padding: 0; display: flex; flex-direction: column; gap: 7px; }
.hours-card__list li { display: flex; justify-content: space-between; gap: 12px; font-size: .82rem; padding: 3px 8px; border-radius: 8px; }
.hours-card__list span { color: var(--ink-dim); }
.hours-card__list strong { font-weight: 700; }
.hours-card__list li.is-today { background: rgba(169, 131, 42, .1); }
.hours-card__list li.is-today span,
.hours-card__list li.is-today strong { color: var(--gold-deep); }

.social { display: flex; gap: 10px; }
.social__link {
  width: 42px; height: 42px; border-radius: 12px;
  background: #fff; border: 1px solid rgba(23, 18, 8, .08); color: var(--ink);
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 16px 32px -26px rgba(23, 18, 8, .28);
  transition: transform .2s ease, background .2s ease, color .2s ease;
}
.social__link:hover { transform: translateY(-2px); background: var(--gold); color: #fff; }

@media (max-width: 860px) {
  .contact__grid { grid-template-columns: 1fr; }
  .field input, .field textarea { padding-left: 38px; }
}

@media (prefers-reduced-motion: reduce) {
  .contact__grid { opacity: 1 !important; transform: none !important; transition: none !important; }
  .status-card.is-open .status-card__dot { animation: none; }
  .submit .spin { animation: none; }
}
</style>