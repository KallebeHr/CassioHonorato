<template>
  <footer class="footer">
    <div class="footer__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="footer__inner">
      <div class="footer__top">
        <div class="brand">
          <div class="brand__mark">
            <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M12 5.5c-1.4-1.6-3-2-4.3-1.6C5.6 4.4 4.5 6.1 4.5 8.3c0 3.7 2.2 6 3.4 9.4.4 1.2 1 2.3 1.7 2.3.9 0 1-1.6 1.2-2.9.2-1.2.5-2.3 1.2-2.3s1 1.1 1.2 2.3c.2 1.3.3 2.9 1.2 2.9.7 0 1.3-1.1 1.7-2.3 1.2-3.4 3.4-5.7 3.4-9.4 0-2.2-1.1-3.9-3.2-4.4-1.3-.4-2.9 0-4.3 1.6Z" />
            </svg>
          </div>
          <div>
            <p class="brand__name">Cássio Honorato | CIRURGIÃO-DENTISTA</p>
            <p class="brand__tag">Odontologia com cuidado e transparência</p>
          </div>
        </div>

        <nav class="footer__nav" aria-label="Navegação do rodapé">
          <p class="footer__col-title">Navegação</p>
          <a v-for="link in navLinks" :key="link.href" :href="link.href">{{ link.label }}</a>
        </nav>

        <div class="footer__contact">
          <p class="footer__col-title">Contato</p>
          <a :href="`tel:${clinic.phoneRaw}`" class="footer__contact-item">
            <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z" /></svg>
            {{ clinic.phone }}
          </a>
          <a :href="`mailto:${clinic.email}`" class="footer__contact-item">
            <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2" /><path d="m22 6-10 7L2 6" /></svg>
            {{ clinic.email }}
          </a>
          <p class="footer__contact-item footer__contact-item--static">
            <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 21s7-6.4 7-12a7 7 0 1 0-14 0c0 5.6 7 12 7 12Z" /><circle cx="12" cy="9" r="2.4" /></svg>
            {{ clinic.address }}
          </p>
        </div>

        <div class="footer__social">
          <p class="footer__col-title">Redes</p>
          <div class="social-row">
            <a :href="clinic.instagramHref" target="_blank" rel="noopener" class="social-btn" aria-label="Instagram">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5" /><circle cx="12" cy="12" r="4" /><circle cx="17.5" cy="6.5" r="1" /></svg>
            </a>
            <a :href="`https://wa.me/${clinic.whatsappRaw}`" target="_blank" rel="noopener" class="social-btn" aria-label="WhatsApp">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z" /></svg>
            </a>
          </div>

          <button type="button" class="top-btn" @click="scrollToTop">
            Voltar ao topo
            <svg viewBox="0 0 24 24" width="13" height="13" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M12 19V5M5 12l7-7 7 7" /></svg>
          </button>
        </div>
      </div>

      <div class="footer__divider"></div>

      <div class="footer__bottom">
        <p class="footer__copy">© {{ year }} Clínica Sorriso — Todos os direitos reservados.</p>
        <button type="button" class="credit" @click="openModal">
          Desenvolvido por <span class="credit__handle">@KallebeMax</span>
        </button>
      </div>
    </div>

    <!-- Modal do desenvolvedor -->
    <Teleport to="body">
      <Transition name="modal">
        <div v-if="modalOpen" class="dev-overlay" @click.self="closeModal">
          <div class="dev-modal" role="dialog" aria-modal="true" aria-labelledby="dev-modal-title">
            <button type="button" class="dev-modal__close" @click="closeModal" aria-label="Fechar">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18M6 6l12 12" /></svg>
            </button>

            <div class="dev-modal__head">
              <div class="dev-avatar">KM</div>
              <div>
                <p id="dev-modal-title" class="dev-name">Kallebe Max</p>
                <p class="dev-role">Desenvolvedor Full Stack</p>
              </div>
            </div>

            <p class="dev-bio">
              Curtiu o site? Eu desenvolvo experiências web do zero — do design à lógica que roda por trás,
              como esse mapa, o formulário e esse modal aqui. Se quiser conversar sobre um projeto (ou só mandar
              aquele feedback sincero), escolhe uma opção abaixo 👇
            </p>

            <div class="stack-row">
              <span v-for="s in stack" :key="s" class="stack-pill">{{ s }}</span>
            </div>

            <div class="rating-row">
              <p class="rating-label">O que achou do site?</p>
              <div class="rating-options">
                <button
                  v-for="r in ratings"
                  :key="r.id"
                  type="button"
                  class="rating-btn"
                  :class="{ 'is-active': rating === r.id }"
                  @click="rating = r.id"
                >
                  <span class="rating-emoji">{{ r.emoji }}</span>
                  {{ r.label }}
                </button>
              </div>
            </div>

            <textarea
              v-model="feedback"
              class="dev-textarea"
              rows="2"
              placeholder="Quer deixar um comentário? (opcional)"
            ></textarea>

            <div class="dev-actions">
              <button type="button" class="dev-cta dev-cta--whats" @click="sendFeedback">
                <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z" /></svg>
                {{ rating || feedback ? 'Enviar feedback' : 'Chamar no WhatsApp' }}
              </button>
              <a :href="devInstagram" target="_blank" rel="noopener" class="dev-cta dev-cta--insta">
                <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5" /><circle cx="12" cy="12" r="4" /><circle cx="17.5" cy="6.5" r="1" /></svg>
                Ver Instagram
              </a>
            </div>

            <p class="dev-foot">Disponível para novos projetos freelance · resposta rápida</p>
          </div>
        </div>
      </Transition>
    </Teleport>
  </footer>
</template>

<script setup>
import { ref, computed, onBeforeUnmount } from 'vue'

/* ---------- Dados da clínica ---------- */
const clinic = {
  phone: '(86) 98147-5263',
  phoneRaw: '5586981475263',
  email: 'contato@cassiohonorato.com.br',
  address: 'Av. Getúlio Vargas, 1240 — Centro, Pedro II - PI',
  instagramHref: 'https://instagram.com/cassio.honorato_',
  whatsappRaw: '5586981475263'
}

const navLinks = [
  { href: '#sobre', label: 'Sobre' },
  { href: '#servicos', label: 'Serviços' },
  { href: '#resultados', label: 'Resultados' },
  { href: '#consultorio', label: 'Consultorio' },
  { href: '#comochegar', label: 'Como Chegar' },
  { href: '#contato', label: 'Contato' }
]

const year = new Date().getFullYear()

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

/* ---------- Dados do desenvolvedor ---------- */
const devWhatsappRaw = '5586988880000'
const devInstagram = 'https://instagram.com/kallebemax'
const stack = ['Vue.js', 'Node.js', 'JavaScript', 'UI/UX', 'APIs REST']

const ratings = [
  { id: 'amei', emoji: '🤩', label: 'Amei' },
  { id: 'legal', emoji: '🙂', label: 'Legal' },
  { id: 'melhorar', emoji: '🤔', label: 'Pode melhorar' }
]

/* ---------- Modal ---------- */
const modalOpen = ref(false)
const rating = ref('')
const feedback = ref('')

function openModal() {
  modalOpen.value = true
  document.body.style.overflow = 'hidden'
  window.addEventListener('keydown', onKeydown)
}
function closeModal() {
  modalOpen.value = false
  document.body.style.overflow = ''
  window.removeEventListener('keydown', onKeydown)
}
function onKeydown(e) {
  if (e.key === 'Escape') closeModal()
}
onBeforeUnmount(() => {
  document.body.style.overflow = ''
  window.removeEventListener('keydown', onKeydown)
})

const ratingLabel = computed(() => ratings.find((r) => r.id === rating.value)?.label || '')

function sendFeedback() {
  const lines = ['Oi Kallebe! Vi o site que você desenvolveu e quero falar com você.']
  if (ratingLabel.value) lines.push(`Achei o site: ${ratingLabel.value}`)
  if (feedback.value.trim()) lines.push('', feedback.value.trim())

  window.open(
    `https://wa.me/${devWhatsappRaw}?text=${encodeURIComponent(lines.join('\n'))}`,
    '_blank',
    'noopener'
  )
  closeModal()
  rating.value = ''
  feedback.value = ''
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.footer {
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
  background: var(--ink);
  color: #f3ede0;
  font-family: 'Manrope', sans-serif;
  padding: clamp(52px, 7vw, 84px) clamp(20px, 6vw, 64px) clamp(24px, 3vw, 32px);
}

.footer__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 34vw; height: 34vw; max-width: 480px; max-height: 480px;
  bottom: -14%; left: -10%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.25), transparent 70%);
  filter: blur(60px);
}

.footer__inner { position: relative; z-index: 2; max-width: 1080px; margin: 0 auto; }

.footer__top {
  display: grid;
  grid-template-columns: 1.4fr 0.8fr 1fr 0.9fr;
  gap: clamp(28px, 4vw, 40px);
}

.brand { display: flex; gap: 12px; align-items: flex-start; }
.brand__mark {
  width: 38px; height: 38px; border-radius: 10px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  color: #241a08;
}
.brand__name { font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.15rem; margin: 0 0 4px; color: #fff; }
.brand__tag { font-size: .82rem; line-height: 1.5; color: rgba(243, 237, 224, .55); margin: 0; max-width: 26ch; }

.footer__col-title {
  font-size: .72rem; letter-spacing: .1em; text-transform: uppercase; font-weight: 700;
  color: var(--gold-soft); margin: 0 0 14px;
}

.footer__nav { display: flex; flex-direction: column; }
.footer__nav a {
  color: rgba(243, 237, 224, .72); text-decoration: none; font-size: .88rem;
  padding: 6px 0; transition: color .2s ease, transform .2s ease; width: fit-content;
}
.footer__nav a:hover { color: var(--gold-soft); transform: translateX(2px); }

.footer__contact { display: flex; flex-direction: column; }
.footer__contact-item {
  display: flex; align-items: flex-start; gap: 8px;
  color: rgba(243, 237, 224, .72); text-decoration: none; font-size: .86rem;
  padding: 6px 0; line-height: 1.4; transition: color .2s ease;
}
.footer__contact-item svg { flex-shrink: 0; margin-top: 2px; color: var(--gold-soft); }
.footer__contact-item:not(.footer__contact-item--static):hover { color: var(--gold-soft); }

.footer__social { display: flex; flex-direction: column; align-items: flex-start; }
.social-row { display: flex; gap: 10px; margin-bottom: 20px; }
.social-btn {
  width: 38px; height: 38px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  background: rgba(243, 237, 224, .06); border: 1px solid rgba(243, 237, 224, .12);
  color: #f3ede0; transition: background .2s ease, color .2s ease, transform .2s ease;
}
.social-btn:hover { background: var(--gold); color: #241a08; transform: translateY(-2px); }

.top-btn {
  display: inline-flex; align-items: center; gap: 6px;
  background: none; border: 1px solid rgba(243, 237, 224, .16); border-radius: 999px;
  color: rgba(243, 237, 224, .75); font-size: .78rem; font-weight: 600;
  padding: 8px 14px; cursor: pointer; transition: border-color .2s ease, color .2s ease;
  font-family: 'Manrope', sans-serif;
}
.top-btn:hover { border-color: var(--gold-soft); color: var(--gold-soft); }

.footer__divider { height: 1px; background: rgba(243, 237, 224, .1); margin: clamp(32px, 4vw, 44px) 0 20px; }

.footer__bottom {
  display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 12px;
}
.footer__copy { margin: 0; font-size: .78rem; color: rgba(243, 237, 224, .5); }

.credit {
  background: none; border: none; cursor: pointer;
  font-family: 'Manrope', sans-serif; font-size: .78rem; color: rgba(243, 237, 224, .5);
  padding: 4px 2px; display: inline-flex; align-items: center; gap: 4px;
  transition: color .2s ease;
}
.credit:hover { color: var(--gold-soft); }
.credit__handle {
  font-weight: 700; color: var(--gold-soft);
  background: linear-gradient(90deg, var(--gold-soft), var(--gold-bright), var(--gold-soft));
  background-size: 200% 100%;
  -webkit-background-clip: text; background-clip: text;
  transition: background-position .4s ease;
}
.credit:hover .credit__handle { background-position: 100% 0; }

/* ---------- Modal do dev ---------- */
.dev-overlay {
  position: fixed; inset: 0; z-index: 999;
  background: rgba(23, 18, 8, .55); backdrop-filter: blur(4px);
  display: flex; align-items: center; justify-content: center;
  padding: 20px;
}

.dev-modal {
  --paper: #faf9f5; --ink: #171208; --ink-dim: rgba(23,18,8,.6);
  --gold: #a9832a; --gold-bright: #cf9f3c; --gold-soft: #e4c674; --gold-deep: #6d4f16;
  position: relative;
  width: 100%; max-width: 420px;
  background: #fff;
  border-radius: 22px;
  padding: clamp(26px, 4vw, 32px);
  box-shadow: 0 40px 80px -30px rgba(23, 18, 8, .5);
  font-family: 'Manrope', sans-serif;
  color: var(--ink);
  max-height: 90vh;
  overflow-y: auto;
}

.dev-modal__close {
  position: absolute; top: 16px; right: 16px;
  width: 30px; height: 30px; border-radius: 50%;
  background: #f4f2ea; border: none; color: var(--ink-dim); cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  transition: background .2s ease, color .2s ease;
}
.dev-modal__close:hover { background: var(--gold); color: #fff; }

.dev-modal__head { display: flex; align-items: center; gap: 14px; margin-bottom: 16px; }
.dev-avatar {
  width: 52px; height: 52px; border-radius: 14px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  background: linear-gradient(150deg, var(--gold-soft), var(--gold-deep));
  color: #fff8ea; font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.1rem;
  box-shadow: 0 10px 22px -10px rgba(109, 79, 22, .5);
}
.dev-name { margin: 0; font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.1rem; }
.dev-role { margin: 2px 0 0; font-size: .8rem; color: var(--gold-deep); font-weight: 700; }

.dev-bio { font-size: .86rem; line-height: 1.6; color: var(--ink-dim); margin: 0 0 16px; }

.stack-row { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 20px; }
.stack-pill {
  font-size: .7rem; font-weight: 700; letter-spacing: .01em;
  padding: 5px 10px; border-radius: 999px;
  background: rgba(169, 131, 42, .1); color: var(--gold-deep);
}

.rating-row { margin-bottom: 14px; }
.rating-label { margin: 0 0 8px; font-size: .78rem; font-weight: 700; color: var(--ink-dim); }
.rating-options { display: flex; gap: 8px; flex-wrap: wrap; }
.rating-btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 7px 12px; border-radius: 999px; border: 1px solid rgba(23, 18, 8, .12);
  background: none; color: var(--ink-dim); font-size: .78rem; font-weight: 600;
  cursor: pointer; transition: all .2s ease; font-family: 'Manrope', sans-serif;
}
.rating-btn.is-active {
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  border-color: transparent; color: #241a08;
}
.rating-emoji { font-size: .95rem; }

.dev-textarea {
  width: 100%; font-family: 'Manrope', sans-serif; font-size: .86rem;
  padding: 10px 13px; border-radius: 12px; border: 1px solid rgba(23, 18, 8, .14);
  background: #fbfaf6; color: var(--ink); resize: vertical; outline: none;
  margin-bottom: 16px; transition: border-color .2s ease, box-shadow .2s ease;
}
.dev-textarea:focus { border-color: var(--gold); box-shadow: 0 0 0 3px rgba(169, 131, 42, .14); }

.dev-actions { display: flex; gap: 10px; flex-wrap: wrap; }
.dev-cta {
  flex: 1 1 160px; display: inline-flex; align-items: center; justify-content: center; gap: 8px;
  padding: 12px 16px; border-radius: 12px; font-weight: 700; font-size: .86rem;
  text-decoration: none; cursor: pointer; border: none;
  transition: transform .2s ease, box-shadow .2s ease;
  font-family: 'Manrope', sans-serif;
}
.dev-cta:hover { transform: translateY(-1px); }
.dev-cta--whats { background: #25d366; color: #fff; box-shadow: 0 14px 26px -14px rgba(37, 211, 102, .55); }
.dev-cta--insta {
  background: linear-gradient(135deg, #f9ce34, #ee2a7b, #6228d7);
  color: #fff; box-shadow: 0 14px 26px -14px rgba(238, 42, 123, .45);
}

.dev-foot { margin: 16px 0 0; text-align: center; font-size: .74rem; color: var(--ink-dim); }

/* Transições do modal */
.modal-enter-active, .modal-leave-active { transition: opacity .25s ease; }
.modal-enter-from, .modal-leave-to { opacity: 0; }
.modal-enter-active .dev-modal, .modal-leave-active .dev-modal { transition: transform .25s ease, opacity .25s ease; }
.modal-enter-from .dev-modal, .modal-leave-to .dev-modal { transform: translateY(14px) scale(.97); opacity: 0; }

@media (max-width: 860px) {
  .footer__top { grid-template-columns: 1fr 1fr; }
  .brand { grid-column: 1 / -1; }
}
@media (max-width: 560px) {
  .footer__top { grid-template-columns: 1fr; gap: 26px; }
  .footer__bottom { flex-direction: column; align-items: flex-start; }
}

@media (prefers-reduced-motion: reduce) {
  .modal-enter-active, .modal-leave-active,
  .modal-enter-active .dev-modal, .modal-leave-active .dev-modal { transition: none; }
}
</style>