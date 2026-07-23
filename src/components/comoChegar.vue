<template>
  <section class="location" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="location__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="location__inner">
      <div class="location__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Como chegar</p>
        </div>
        <h2 class="headline">Estamos <span class="word--gold">pertinho</span> de você</h2>
        <p class="subtext">
          Encontre o melhor caminho até a clínica — de carro, a pé ou de transporte público.
        </p>
      </div>

      <div class="location__grid">
        <!-- Painel de informações -->
        <aside class="info">
          <div class="info__card">
            <p class="info__tag">Endereço</p>
            <p class="info__address">{{ clinic.address }}</p>

            <button class="info__copy" @click="copyAddress" type="button">
              <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect v-if="!copied" x="9" y="9" width="11" height="11" rx="2" />
                <path v-if="!copied" d="M5 15V5a2 2 0 0 1 2-2h10" />
                <path v-else d="M5 12l4 4 10-10" />
              </svg>
              {{ copied ? 'Endereço copiado!' : 'Copiar endereço' }}
            </button>

            <ul class="info__hours">
              <li v-for="h in clinic.hours" :key="h.day">
                <span>{{ h.day }}</span>
                <strong>{{ h.time }}</strong>
              </li>
            </ul>

            <div class="info__contact">
              <a :href="`tel:${clinic.phoneRaw}`" class="contact-btn">
                <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z" />
                </svg>
                {{ clinic.phone }}
              </a>
              <a :href="whatsappLink" target="_blank" rel="noopener" class="contact-btn contact-btn--whats">
                <svg viewBox="0 0 24 24" width="14" height="14" fill="currentColor">
                  <path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z" />
                </svg>
                WhatsApp
              </a>
            </div>
          </div>

          <div class="transport">
            <p class="transport__label">Como prefere ir?</p>
            <div class="transport__tabs" role="tablist">
              <button
                v-for="m in transportModes"
                :key="m.id"
                type="button"
                class="transport__tab"
                :class="{ 'is-active': mode === m.id }"
                @click="mode = m.id"
              >
                <span v-html="m.icon" class="transport__icon"></span>
                {{ m.label }}
              </button>
            </div>

            <button class="cta" type="button" @click="openDirections">
              Traçar rota até nós
              <svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M5 12h14M13 6l6 6-6 6" />
              </svg>
            </button>

            <p class="distance-note" v-if="distanceLabel">
              <span class="distance-dot"></span> {{ distanceLabel }}
            </p>
            <p class="distance-note distance-note--muted" v-else-if="locateError">
              {{ locateError }}
            </p>
          </div>
        </aside>

        <!-- Mapa -->
        <div class="map__frame">
          <div class="map__controls">
            <button type="button" class="map__btn" @click="locateMe" :disabled="locating" title="Usar minha localização" aria-label="Usar minha localização">
              <svg v-if="!locating" viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="3" />
                <path d="M12 2v3M12 19v3M22 12h-3M5 12H2" />
              </svg>
              <svg v-else class="spin" viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
                <path d="M12 2a10 10 0 0 1 10 10" />
              </svg>
            </button>
            <div class="map__zoom">
              <button type="button" class="map__btn" @click="zoomIn" aria-label="Aumentar zoom">+</button>
              <button type="button" class="map__btn" @click="zoomOut" aria-label="Diminuir zoom">–</button>
            </div>
            <button type="button" class="map__btn" @click="toggleFullscreen" title="Tela cheia" aria-label="Tela cheia">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M8 3H5a2 2 0 0 0-2 2v3M16 3h3a2 2 0 0 1 2 2v3M21 16v3a2 2 0 0 1-2 2h-3M8 21H5a2 2 0 0 1-2-2v-3" />
              </svg>
            </button>
          </div>

          <div ref="mapWrapper" class="map__wrapper">
            <div ref="mapEl" class="map__canvas"></div>
          </div>

          <span class="map__badge">
            <span class="map__badge-dot"></span> {{ clinic.name }}
          </span>

          <p v-if="routeInfo" class="map__route-info">{{ routeInfo }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
/**
 * Componente de localização/mapa.
 *
 * Dependências (npm):
 *   npm install leaflet leaflet-routing-machine
 *
 * - Usa OpenStreetMap + CartoDB Voyager (tiles gratuitos, sem API key).
 * - Traça rota real com leaflet-routing-machine (usa o servidor público OSRM);
 *   se a lib não estiver instalada ou o serviço falhar, o componente degrada
 *   graciosamente e usa apenas o link "Como chegar" do Google Maps.
 * - Geolocalização é opcional: sem permissão, tudo continua funcionando,
 *   só não mostra distância/rota.
 */
import { ref, reactive, computed, onMounted, onBeforeUnmount } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const sectionRef = ref(null)
const mapWrapper = ref(null)
const mapEl = ref(null)
const visible = ref(false)

/* ---------- Dados da clínica — troque pelos seus ---------- */
const clinic = reactive({
  name: 'Cássio Honorato | CIRURGIÃO-DENTISTA',
  address: 'Av. Getúlio Vargas, 1240 — Centro, Pedro II - PI',
  lat: -2.9055,
  lng: -41.7767,
  phone: '(86) 98147-5263',
  phoneRaw: '5586981475263',
  whatsappMsg: 'Olá! Gostaria de agendar uma avaliação.',
  hours: [
    { day: 'Seg – Sex', time: '08h às 19h' },
    { day: 'Sábado', time: '08h às 13h' },
    { day: 'Domingo', time: 'Fechado' }
  ]
})

const whatsappLink = computed(
  () => `https://wa.me/${clinic.phoneRaw}?text=${encodeURIComponent(clinic.whatsappMsg)}`
)

/* ---------- Transporte ---------- */
const transportModes = [
  { id: 'driving', label: 'Carro', gmaps: 'driving', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 17h14M5 17a2 2 0 1 1-4 0 2 2 0 0 1 4 0Zm14 0a2 2 0 1 1-4 0 2 2 0 0 1 4 0ZM3 17V9l2-5h14l2 5v8"/></svg>' },
  { id: 'walking', label: 'A pé', gmaps: 'walking', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="13" cy="4" r="1.6"/><path d="M10 21l1.6-6.4L9 13l1-5 3.5 1L15 11l3 1M8 14l-2 7"/></svg>' },
  { id: 'transit', label: 'Ônibus', gmaps: 'transit', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="13" rx="2"/><path d="M4 11h16M8 19l-1.5 2M16 19l1.5 2"/><circle cx="8" cy="15" r=".6"/><circle cx="16" cy="15" r=".6"/></svg>' },
  { id: 'bicycling', label: 'Bike', gmaps: 'bicycling', icon: '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="6" cy="17" r="3"/><circle cx="18" cy="17" r="3"/><path d="M6 17l4-8h5l3 8M10 9H8m5-3h3l2 5"/></svg>' }
]
const mode = ref('driving')

/* ---------- Estado ---------- */
let map = null
let clinicMarker = null
let userMarker = null
let routingControl = null

const locating = ref(false)
const locateError = ref('')
const userCoords = ref(null)
const distanceLabel = ref('')
const routeInfo = ref('')

/* ---------- Ícones customizados (sem depender de imagens externas) ---------- */
function pinIcon() {
  return L.divIcon({
    className: 'map-pin',
    html: `
      <span class="map-pin__pulse"></span>
      <span class="map-pin__dot">
        <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="#241a08" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 21s7-6.4 7-12a7 7 0 1 0-14 0c0 5.6 7 12 7 12Z"/>
          <circle cx="12" cy="9" r="2.4"/>
        </svg>
      </span>`,
    iconSize: [36, 46],
    iconAnchor: [18, 42],
    popupAnchor: [0, -40]
  })
}

function userIcon() {
  return L.divIcon({
    className: 'map-user',
    html: `<span class="map-user__pulse"></span><span class="map-user__dot"></span>`,
    iconSize: [18, 18],
    iconAnchor: [9, 9]
  })
}

/* ---------- Mapa ---------- */
function initMap() {
  if (!mapEl.value) return

  map = L.map(mapEl.value, {
    zoomControl: false,
    scrollWheelZoom: false,
    center: [clinic.lat, clinic.lng],
    zoom: 15
  })

  map.on('focus', () => map.scrollWheelZoom.enable())
  map.on('blur', () => map.scrollWheelZoom.disable())

  L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; OpenStreetMap contributors &copy; CARTO',
    maxZoom: 19
  }).addTo(map)

  clinicMarker = L.marker([clinic.lat, clinic.lng], { icon: pinIcon(), keyboard: false })
    .addTo(map)
    .bindPopup(
      `<strong>${clinic.name}</strong><br>${clinic.address}`,
      { closeButton: false, offset: [0, -6] }
    )

  setTimeout(() => map.invalidateSize(), 250)
}

function zoomIn() { map?.zoomIn() }
function zoomOut() { map?.zoomOut() }

function toggleFullscreen() {
  const el = mapWrapper.value
  if (!el) return
  if (!document.fullscreenElement) {
    el.requestFullscreen?.().then(() => setTimeout(() => map?.invalidateSize(), 200))
  } else {
    document.exitFullscreen?.().then(() => setTimeout(() => map?.invalidateSize(), 200))
  }
}

/* ---------- Geolocalização + distância ---------- */
function haversine(lat1, lng1, lat2, lng2) {
  const R = 6371
  const dLat = (lat2 - lat1) * Math.PI / 180
  const dLng = (lng2 - lng1) * Math.PI / 180
  const a =
    Math.sin(dLat / 2) ** 2 +
    Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLng / 2) ** 2
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a))
}

function locateMe() {
  if (!('geolocation' in navigator)) {
    locateError.value = 'Seu navegador não permite localização automática.'
    return
  }
  locating.value = true
  locateError.value = ''

  navigator.geolocation.getCurrentPosition(
    (pos) => {
      locating.value = false
      const { latitude, longitude } = pos.coords
      userCoords.value = { lat: latitude, lng: longitude }

      if (userMarker) {
        userMarker.setLatLng([latitude, longitude])
      } else {
        userMarker = L.marker([latitude, longitude], { icon: userIcon(), keyboard: false }).addTo(map)
      }

      const km = haversine(latitude, longitude, clinic.lat, clinic.lng)
      distanceLabel.value = km < 1
        ? `Você está a ${Math.round(km * 1000)} m da clínica`
        : `Você está a ${km.toFixed(1).replace('.', ',')} km da clínica`

      const bounds = L.latLngBounds([[latitude, longitude], [clinic.lat, clinic.lng]])
      map.fitBounds(bounds, { padding: [56, 56], maxZoom: 16 })

      drawRoute()
    },
    () => {
      locating.value = false
      locateError.value = 'Não foi possível acessar sua localização.'
    },
    { enableHighAccuracy: true, timeout: 8000 }
  )
}

/* ---------- Rota (opcional, via leaflet-routing-machine) ---------- */
async function drawRoute() {
  if (!userCoords.value || !map) return
  try {
    const mod = await import('leaflet-routing-machine')
    await import('leaflet-routing-machine/dist/leaflet-routing-machine.css')
    const Routing = mod.default?.Routing || L.Routing

    if (routingControl) {
      map.removeControl(routingControl)
      routingControl = null
    }

    routingControl = Routing.control({
      waypoints: [
        L.latLng(userCoords.value.lat, userCoords.value.lng),
        L.latLng(clinic.lat, clinic.lng)
      ],
      lineOptions: { styles: [{ color: '#a9832a', weight: 5, opacity: 0.85 }] },
      addWaypoints: false,
      draggableWaypoints: false,
      fitSelectedRoutes: true,
      show: false,
      createMarker: () => null
    }).addTo(map)

    routingControl.on('routesfound', (e) => {
      const r = e.routes[0]
      const mins = Math.round(r.summary.totalTime / 60)
      const km = (r.summary.totalDistance / 1000).toFixed(1).replace('.', ',')
      routeInfo.value = `Rota traçada · ${km} km · ~${mins} min`
    })
    routingControl.on('routingerror', () => { routeInfo.value = '' })
  } catch {
    // leaflet-routing-machine não instalado ou serviço indisponível — sem problema,
    // o botão "Traçar rota" continua funcionando via Google Maps.
    routeInfo.value = ''
  }
}

/* ---------- Abrir direções no Google Maps ---------- */
function openDirections() {
  const dest = `${clinic.lat},${clinic.lng}`
  const params = new URLSearchParams({
    api: '1',
    destination: dest,
    travelmode: transportModes.find((m) => m.id === mode.value)?.gmaps || 'driving'
  })
  if (userCoords.value) {
    params.set('origin', `${userCoords.value.lat},${userCoords.value.lng}`)
  }
  window.open(`https://www.google.com/maps/dir/?${params.toString()}`, '_blank', 'noopener')
}

/* ---------- Copiar endereço ---------- */
const copied = ref(false)
async function copyAddress() {
  try {
    await navigator.clipboard.writeText(clinic.address)
  } catch {
    const ta = document.createElement('textarea')
    ta.value = clinic.address
    document.body.appendChild(ta)
    ta.select()
    document.execCommand('copy')
    document.body.removeChild(ta)
  }
  copied.value = true
  setTimeout(() => (copied.value = false), 2000)
}

/* ---------- Reveal on scroll ---------- */
let observer = null
onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches

  initMap()

  if (prefersReducedMotion || !sectionRef.value) {
    visible.value = true
  } else {
    observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          visible.value = true
          map?.invalidateSize()
          observer.disconnect()
        }
      },
      { threshold: 0.1 }
    )
    observer.observe(sectionRef.value)
  }

  window.addEventListener('resize', handleResize)
})

function handleResize() { map?.invalidateSize() }

onBeforeUnmount(() => {
  observer?.disconnect()
  window.removeEventListener('resize', handleResize)
  map?.remove()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;0,9..144,600;1,9..144,500;1,9..144,600&family=Manrope:wght@400;500;600;700&display=swap');

.location {
  --paper: #f5f3ec;
  --ink: #171208;
  --ink-dim: rgba(23, 18, 8, 0.6);
  --gold: #a9832a;
  --gold-bright: #cf9f3c;
  --gold-soft: #e4c674;
  --gold-deep: #6d4f16;
  --gold-line: rgba(169, 131, 42, 0.28);

  position: relative;
  overflow: hidden;
  background: #F9F8F3;

  color: var(--ink);
  font-family: 'Manrope', sans-serif;
  padding: clamp(64px, 9vw, 120px) clamp(20px, 6vw, 64px);
}

.location__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 30vw; height: 30vw; max-width: 420px; max-height: 420px;
  bottom: -10%; right: -8%;
  background: radial-gradient(circle, rgba(169, 131, 42, 0.16), transparent 70%);
  filter: blur(50px);
  mix-blend-mode: multiply;
}

.location__inner { position: relative; z-index: 2; max-width: 1080px; margin: 0 auto; }

.location__head { text-align: center; max-width: 620px; margin: 0 auto clamp(36px, 5vw, 52px); }
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

.location__grid {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: clamp(24px, 3.5vw, 48px);
  align-items: stretch;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.location.is-visible .location__grid { opacity: 1; transform: none; }

/* ---------- Painel de info ---------- */
.info { display: flex; flex-direction: column; gap: 18px; }

.info__card {
  background: #fff;
  border: 1px solid rgba(23, 18, 8, .08);
  border-radius: 16px;
  padding: clamp(20px, 2.6vw, 28px);
  box-shadow: 0 20px 40px -28px rgba(23, 18, 8, .28);
}
.info__tag { font-size: .74rem; letter-spacing: .06em; text-transform: uppercase; color: var(--gold-deep); font-weight: 700; margin: 0 0 6px; }
.info__address { font-family: 'Fraunces', serif; font-weight: 600; font-size: 1.1rem; line-height: 1.4; margin: 0 0 14px; }

.info__copy {
  display: inline-flex; align-items: center; gap: 7px;
  background: none; border: 1px solid var(--gold-line); border-radius: 999px;
  padding: 7px 14px; font-size: .78rem; font-weight: 600; color: var(--gold-deep);
  cursor: pointer; transition: background .2s ease, color .2s ease, border-color .2s ease;
  font-family: 'Manrope', sans-serif;
}
.info__copy:hover { background: var(--gold); border-color: var(--gold); color: #fff; }

.info__hours {
  list-style: none; margin: 18px 0 0; padding: 14px 0 0;
  border-top: 1px solid rgba(23, 18, 8, .08);
  display: flex; flex-direction: column; gap: 7px;
}
.info__hours li { display: flex; justify-content: space-between; gap: 12px; font-size: .84rem; }
.info__hours span { color: var(--ink-dim); }
.info__hours strong { font-weight: 700; }

.info__contact { display: flex; gap: 10px; margin-top: 18px; flex-wrap: wrap; }
.contact-btn {
  flex: 1 1 auto; display: inline-flex; align-items: center; justify-content: center; gap: 7px;
  padding: 10px 14px; border-radius: 10px; font-size: .82rem; font-weight: 700;
  text-decoration: none; color: var(--ink); background: #f4f2ea;
  border: 1px solid rgba(23, 18, 8, .08); transition: transform .2s ease, background .2s ease;
  white-space: nowrap;
}
.contact-btn:hover { transform: translateY(-1px); background: #eee9da; }
.contact-btn--whats { background: #25d366; color: #fff; border-color: transparent; }
.contact-btn--whats:hover { background: #20bd5a; }

.transport {
  background: #fff;
  border: 1px solid rgba(23, 18, 8, .08);
  border-radius: 16px;
  padding: clamp(20px, 2.6vw, 28px);
  box-shadow: 0 20px 40px -28px rgba(23, 18, 8, .28);
}
.transport__label { font-size: .78rem; font-weight: 700; color: var(--ink-dim); margin: 0 0 12px; }
.transport__tabs { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 18px; }
.transport__tab {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 13px; border-radius: 999px; border: 1px solid rgba(23, 18, 8, .12);
  background: none; color: var(--ink-dim); font-size: .78rem; font-weight: 600;
  cursor: pointer; transition: all .2s ease; font-family: 'Manrope', sans-serif;
}
.transport__tab.is-active {
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  border-color: transparent; color: #241a08;
}
.transport__icon { display: inline-flex; }

.cta {
  width: 100%; display: inline-flex; align-items: center; justify-content: center; gap: 8px;
  padding: 13px 18px; border-radius: 12px; border: none; cursor: pointer;
  font-family: 'Manrope', sans-serif; font-weight: 700; font-size: .92rem;
  color: #fff8ea;
  background: linear-gradient(120deg, var(--gold-deep), var(--gold) 55%, var(--gold-bright));
  box-shadow: 0 14px 28px -14px rgba(109, 79, 22, .55);
  transition: transform .2s ease, box-shadow .2s ease;
}
.cta:hover { transform: translateY(-1px); box-shadow: 0 18px 32px -14px rgba(109, 79, 22, .6); }
.cta svg { flex-shrink: 0; }

.distance-note {
  display: flex; align-items: center; gap: 7px;
  margin: 14px 0 0; font-size: .8rem; font-weight: 600; color: var(--gold-deep);
}
.distance-note--muted { color: var(--ink-dim); font-weight: 500; }
.distance-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--gold); flex-shrink: 0; }

/* ---------- Mapa ---------- */
.map__frame {
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  border: 1px solid rgba(23, 18, 8, .08);
  box-shadow: 0 24px 48px -26px rgba(23, 18, 8, .34);
  min-height: 420px;
  background: var(--paper);
}

.map__wrapper { position: absolute; inset: 0; }
.map__canvas { width: 100%; height: 100%; background: #eee6d2; }

/* Aquece a paleta padrão do OpenStreetMap/CARTO para casar com o dourado da marca */
.map__canvas :deep(.leaflet-tile-pane) {
  filter: sepia(12%) saturate(88%) hue-rotate(-6deg) brightness(1.02) contrast(1.03);
}
.map__canvas :deep(.leaflet-popup-content-wrapper) {
  background: #fff; color: var(--ink); border-radius: 12px;
  font-family: 'Manrope', sans-serif; font-size: .82rem;
  box-shadow: 0 14px 30px -14px rgba(23, 18, 8, .4);
}
.map__canvas :deep(.leaflet-popup-content) { margin: 10px 14px; line-height: 1.5; }
.map__canvas :deep(.leaflet-popup-tip) { background: #fff; }
.map__canvas :deep(.leaflet-routing-container) { display: none; }

.map__controls {
  position: absolute; top: 14px; right: 14px; z-index: 5;
  display: flex; flex-direction: column; gap: 8px; align-items: flex-end;
}
.map__zoom { display: flex; flex-direction: column; border-radius: 10px; overflow: hidden; box-shadow: 0 10px 22px -12px rgba(23,18,8,.4); }
.map__btn {
  width: 36px; height: 36px; display: flex; align-items: center; justify-content: center;
  background: rgba(255, 255, 255, .92); backdrop-filter: blur(3px);
  border: none; border-bottom: 1px solid rgba(23,18,8,.08);
  color: var(--ink); font-size: 1.05rem; font-weight: 700; cursor: pointer;
  box-shadow: 0 10px 22px -12px rgba(23,18,8,.4); border-radius: 10px;
  transition: background .2s ease, color .2s ease;
}
.map__zoom .map__btn { border-radius: 0; box-shadow: none; }
.map__zoom .map__btn:last-child { border-bottom: none; }
.map__btn:hover:not(:disabled) { background: var(--gold); color: #fff; }
.map__btn:disabled { opacity: .6; cursor: default; }
.map__btn .spin { animation: spin 1s linear infinite; }
@keyframes spin { to { transform: rotate(360deg); } }

.map__badge {
  position: absolute; left: 14px; bottom: 14px; z-index: 5;
  display: inline-flex; align-items: center; gap: 7px;
  background: rgba(23, 18, 8, .6); backdrop-filter: blur(3px);
  color: #fbf3de; font-size: .72rem; font-weight: 700; letter-spacing: .02em;
  padding: 7px 13px; border-radius: 999px;
}
.map__badge-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--gold-soft); }

.map__route-info {
  position: absolute; right: 14px; bottom: 14px; z-index: 5; margin: 0;
  background: rgba(23, 18, 8, .6); backdrop-filter: blur(3px);
  color: #fbf3de; font-size: .72rem; font-weight: 700;
  padding: 7px 13px; border-radius: 999px;
}

/* Marcador da clínica */
.map__canvas :deep(.map-pin) { position: relative; }
.map__canvas :deep(.map-pin__dot) {
  position: absolute; left: 50%; top: 6px; transform: translateX(-50%);
  width: 30px; height: 30px; border-radius: 50% 50% 50% 0;
  transform: translateX(-50%) rotate(-45deg);
  background: linear-gradient(160deg, var(--gold-soft), var(--gold));
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 8px 16px -6px rgba(23,18,8,.5);
}
.map__canvas :deep(.map-pin__dot svg) { transform: rotate(45deg); }
.map__canvas :deep(.map-pin__pulse) {
  position: absolute; left: 50%; top: 22px; transform: translate(-50%, -50%);
  width: 14px; height: 14px; border-radius: 50%; background: rgba(169, 131, 42, .35);
  animation: pin-pulse 2.2s ease-out infinite;
}
@keyframes pin-pulse {
  0% { transform: translate(-50%, -50%) scale(1); opacity: .7; }
  100% { transform: translate(-50%, -50%) scale(3.4); opacity: 0; }
}

/* Marcador do usuário */
.map__canvas :deep(.map-user__dot) {
  position: absolute; inset: 4px; border-radius: 50%;
  background: #2f6fed; border: 2px solid #fff; box-shadow: 0 2px 8px rgba(0,0,0,.35);
}
.map__canvas :deep(.map-user__pulse) {
  position: absolute; inset: 0; border-radius: 50%; background: rgba(47, 111, 237, .35);
  animation: pin-pulse 2s ease-out infinite;
}

@media (max-width: 860px) {
  .location__grid { grid-template-columns: 1fr; }
  .map__frame { order: -1; min-height: 320px; }
  .info__contact { flex-wrap: wrap; }
}

@media (prefers-reduced-motion: reduce) {
  .location__grid { opacity: 1 !important; transform: none !important; transition: none !important; }
  .map__canvas :deep(.map-pin__pulse),
  .map__canvas :deep(.map-user__pulse) { animation: none; }
  .map__btn .spin { animation: none; }
}
</style>