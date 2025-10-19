<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'

type Mission = {
  id: number
  title: string
  detail: string
  done: boolean
}

type Memory = {
  id: number
  text: string
  timestamp: string
}

const compliments = [
  'Ranum, kamu itu pelangi yang datang setelah hujan. ✨',
  'Hei Ranum! Senyummu bikin dunia Ryan jadi lebih terang. 🌈',
  'Kalau kucing aja bisa punya sembilan nyawa, Ranum punya sembilan puluh juta alasan untuk bahagia! 🐾',
  'Setiap detik Ryan mikirin Ranum, otomatis ada kupu-kupu lucu menari. 🦋',
  'Jangan lupa, Ranum adalah juara bertahan hati Ryan sepanjang masa! 🏆',
  'Ranum kuat banget. Kalau hati kayak game, Ranum sudah level dewa! 🎮',
  'Semangat Ranum itu menular, jadi Ryan ikut senyum terus. 😸'
]

const cozyActivities: Mission[] = [
  {
    id: 1,
    title: 'Tarik napas kucing',
    detail: 'Ambil napas dalam tiga hitungan, keluarkan perlahan kayak kucing peregangan.',
    done: false
  },
  {
    id: 2,
    title: 'Minum air putih favorit',
    detail: 'Ambil gelas lucu Ranum, minum sampai kucing hitam putih ini bilang cukup.',
    done: false
  },
  {
    id: 3,
    title: 'Gerak bahagia 30 detik',
    detail: 'Putar badan, lompat pelan, atau sekadar goyangkan tangan kayak antena kucing.',
    done: false
  },
  {
    id: 4,
    title: 'Catnap kilat',
    detail: 'Pejamkan mata, bayangkan Ryan peluk Ranum sambil dikelilingi kitty hitam putih.',
    done: false
  }
]

const memoryLog = ref<Memory[]>([])
const selectedCompliment = ref(compliments[0])
const floatingHearts = ref<{ id: number; left: number; size: number }[]>([])
const sparkleBursts = ref<{ id: number; left: number; duration: number }[]>([])
const missions = ref([...cozyActivities])
const playlist = [
  'Suara hujan halus di jendela Ryan',
  'Denting piano mini di ruang imajinasi',
  'Kucing mengorok lucu, ritme 60 bpm',
  'Bisikan manis: "Ranum pasti bisa!"'
]
const selectedPlaylist = ref(playlist[0])
const moodLevel = ref(4)
let sparkleId = 0
let heartId = 0
let audioCtx: AudioContext | null = null

const missionProgress = computed(() => {
  const total = missions.value.length
  const done = missions.value.filter((m) => m.done).length
  return Math.round((done / total) * 100)
})

const moodCaption = computed(() => {
  if (moodLevel.value <= 2) {
    return 'Ranum lagi butuh pelukan ekstra, nih. Kitty siap merapat!'
  }
  if (moodLevel.value <= 4) {
    return 'Mood Ranum mulai naik, kayak balon keju mozzarella!'
  }
  if (moodLevel.value <= 6) {
    return 'Udah cerah banget, hampir seterang mata Ryan lihat Ranum.'
  }
  return 'Wah! Ranum super bersinar! Kitty sampe butuh kacamata hitam-putih.'
})

function ensureAudio() {
  if (!audioCtx) {
    audioCtx = new AudioContext()
  }
  if (audioCtx.state === 'suspended') {
    audioCtx.resume()
  }
  return audioCtx
}

function playChime(intensity: 'soft' | 'bright' = 'soft') {
  const ctx = ensureAudio()
  const now = ctx.currentTime
  const oscillator = ctx.createOscillator()
  const gain = ctx.createGain()
  const baseFrequency = intensity === 'soft' ? 540 : 720

  oscillator.type = 'triangle'
  oscillator.frequency.setValueAtTime(baseFrequency, now)
  oscillator.frequency.linearRampToValueAtTime(baseFrequency - 120, now + 0.35)

  gain.gain.setValueAtTime(0.001, now)
  gain.gain.linearRampToValueAtTime(0.2, now + 0.05)
  gain.gain.exponentialRampToValueAtTime(0.0001, now + 1.2)

  oscillator.connect(gain)
  gain.connect(ctx.destination)
  oscillator.start(now)
  oscillator.stop(now + 1.3)
}

function spawnSparkles() {
  for (let i = 0; i < 3; i += 1) {
    const id = sparkleId++
    sparkleBursts.value.push({
      id,
      left: Math.random() * 80 + 10,
      duration: Math.random() * 1 + 1.2
    })
    setTimeout(() => {
      sparkleBursts.value = sparkleBursts.value.filter((sparkle) => sparkle.id !== id)
    }, 1800)
  }
}

function randomCompliment() {
  const randomIndex = Math.floor(Math.random() * compliments.length)
  const random = compliments[randomIndex] ?? compliments[0] ?? 'Ranum selalu hebat di mata Ryan!'
  selectedCompliment.value = random
  memoryLog.value.unshift({
    id: Date.now(),
    text: random,
    timestamp: new Date().toLocaleTimeString('id-ID', {
      hour: '2-digit',
      minute: '2-digit'
    })
  })
  memoryLog.value = memoryLog.value.slice(0, 6)
  playChime('bright')
  spawnSparkles()
}

function toggleMission(id: number) {
  missions.value = missions.value.map((mission) =>
    mission.id === id ? { ...mission, done: !mission.done } : mission
  )
  playChime('soft')
  sendHug()
}

function sendHug() {
  const id = heartId++
  floatingHearts.value.push({
    id,
    left: Math.random() * 70 + 15,
    size: Math.random() * 18 + 22
  })
  setTimeout(() => {
    floatingHearts.value = floatingHearts.value.filter((heart) => heart.id !== id)
  }, 3600)
}

onMounted(() => {
  memoryLog.value = [
    {
      id: Date.now(),
      text: 'Ranum hadir! Kitty sudah siap gelitik dengan semangat manis.',
      timestamp: new Date().toLocaleTimeString('id-ID', {
        hour: '2-digit',
        minute: '2-digit'
      })
    }
  ]
})
</script>

<template>
  <div class="page">
    <div class="floating-hearts" aria-hidden="true">
      <span
        v-for="heart in floatingHearts"
        :key="heart.id"
        class="heart"
        :style="{ left: `${heart.left}%`, fontSize: `${heart.size}px` }"
      >
        ♡
      </span>
    </div>

    <div class="sparkle-layer" aria-hidden="true">
      <span
        v-for="sparkle in sparkleBursts"
        :key="sparkle.id"
        class="sparkle"
        :style="{ left: `${sparkle.left}%`, animationDuration: `${sparkle.duration}s` }"
      ></span>
    </div>

    <header class="hero" aria-live="polite">
      <div class="kitty-emblem" role="img" aria-label="Ikon kucing hitam putih">
        <svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg">
          <circle cx="60" cy="60" r="58" class="emblem-ring" />
          <g class="kitty">
            <path
              class="kitty-face"
              d="M30 70 Q60 110 90 70 Q92 40 78 30 L60 45 L42 30 Q28 40 30 70Z"
            />
            <path class="kitty-mask" d="M60 45 Q78 58 80 74 Q60 88 40 74 Q42 58 60 45Z" />
            <circle class="kitty-eye" cx="48" cy="66" r="6" />
            <circle class="kitty-eye" cx="72" cy="66" r="6" />
            <path class="kitty-nose" d="M58 76 L62 76 L60 80 Z" />
            <path class="kitty-mouth" d="M60 80 Q66 86 72 80" />
            <path class="kitty-mouth" d="M60 80 Q54 86 48 80" />
            <g class="kitty-whiskers">
              <path d="M38 74 L24 70" />
              <path d="M38 80 L22 84" />
              <path d="M82 74 L96 70" />
              <path d="M82 80 L98 84" />
            </g>
          </g>
        </svg>
      </div>
      <div class="hero-text">
        <h1>Ranum, Kitty Guardian datang membawa semangat!</h1>
        <p>
          Hai Ranum, ini Ryan. Aku bikin dunia imut ini khusus buat kamu supaya senyum kamu muncul
          lagi. Setiap klik di sini sama seperti pelukan hangat dari aku.
        </p>
        <button class="main-button" type="button" @click="randomCompliment">
          🎀 Dapatkan pesan semangat baru
        </button>
      </div>
    </header>

    <section class="highlight">
      <h2>Pesan hangat untuk Ranum</h2>
      <p class="highlight-text">{{ selectedCompliment }}</p>
      <div class="mood-control">
        <label for="mood">Skala kilau hati Ranum: {{ moodLevel }}</label>
        <input
          id="mood"
          v-model.number="moodLevel"
          class="slider"
          type="range"
          min="1"
          max="7"
        />
        <p class="mood-caption">{{ moodCaption }}</p>
      </div>
    </section>

    <section class="missions">
      <div class="missions-header">
        <h2>Mini-misi penyemangat</h2>
        <span class="progress-pill">Progress: {{ missionProgress }}%</span>
      </div>
      <ul>
        <li v-for="mission in missions" :key="mission.id">
          <article :class="['mission-card', { done: mission.done }]">
            <header>
              <h3>{{ mission.title }}</h3>
              <button type="button" class="mini-button" @click="toggleMission(mission.id)">
                {{ mission.done ? 'Ulangi lagi' : 'Selesai!'}}
              </button>
            </header>
            <p>{{ mission.detail }}</p>
          </article>
        </li>
      </ul>
    </section>

    <section class="playlist">
      <h2>Suara penyemangat favorit Ranum</h2>
      <div class="options">
        <label v-for="sound in playlist" :key="sound" class="sound-option">
          <input
            type="radio"
            name="sound"
            :value="sound"
            v-model="selectedPlaylist"
            @change="playChime('soft')"
          />
          <span>{{ sound }}</span>
        </label>
      </div>
      <button type="button" class="main-button ghost" @click="sendHug">
        🐾 Kirim pelukan kilat
      </button>
      <p class="playlist-note">
        Saat Ranum klik pelukan, kitty mengirimkan hati dan musik mini. Boleh klik berkali-kali!
      </p>
    </section>

    <section class="memory">
      <h2>Jejak pesan hangat</h2>
      <ol>
        <li v-for="memory in memoryLog" :key="memory.id">
          <time :datetime="memory.timestamp">{{ memory.timestamp }}</time>
          <span>{{ memory.text }}</span>
        </li>
      </ol>
    </section>

    <footer class="footer">
      <p>
        Dibuat penuh cinta oleh Ryan untuk Ranum.
        <span class="footer-kitty" aria-hidden="true">🐱‍⬛</span>
      </p>
      <small>Semua interaksi di sini aman, nyaman, dan penuh kasih.</small>
    </footer>
  </div>
</template>

<style scoped>
:global(body) {
  margin: 0;
  font-family: 'Quicksand', 'Poppins', 'Nunito', system-ui, sans-serif;
  background: radial-gradient(circle at top left, #ffe9f6 0%, #f3f7ff 45%, #fff 100%);
  color: #1f1f2e;
}

.page {
  min-height: 100vh;
  padding: 2.5rem clamp(1.5rem, 4vw, 4rem) 4rem;
  position: relative;
  overflow: hidden;
}

.hero {
  display: grid;
  gap: clamp(1.5rem, 4vw, 3rem);
  grid-template-columns: minmax(260px, 320px) minmax(0, 1fr);
  align-items: center;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.72), rgba(255, 240, 250, 0.82));
  border-radius: 32px;
  padding: clamp(1.75rem, 3vw, 3rem);
  box-shadow: 0 18px 45px rgba(53, 61, 131, 0.12);
  border: 2px solid rgba(0, 0, 0, 0.06);
}

.kitty-emblem {
  width: min(100%, 280px);
  aspect-ratio: 1;
  margin: 0 auto;
  background: linear-gradient(145deg, #fff, #f2f7ff);
  border-radius: 38px;
  display: grid;
  place-items: center;
  box-shadow: inset 0 0 0 6px rgba(255, 255, 255, 0.6), 0 14px 30px rgba(31, 37, 74, 0.18);
}

.kitty-emblem svg {
  width: 85%;
  height: auto;
}

.emblem-ring {
  fill: none;
  stroke: #1f1f2e;
  stroke-width: 5px;
}

.kitty-face {
  fill: #fff;
  stroke: #1f1f2e;
  stroke-width: 4px;
  stroke-linejoin: round;
}

.kitty-mask {
  fill: #1f1f2e;
  stroke: #1f1f2e;
  stroke-width: 2px;
  opacity: 0.94;
}

.kitty-eye {
  fill: #fff;
  stroke: #1f1f2e;
  stroke-width: 2px;
}

.kitty-nose {
  fill: #fff;
  stroke: #1f1f2e;
  stroke-width: 2px;
}

.kitty-mouth {
  fill: none;
  stroke: #1f1f2e;
  stroke-width: 2.5px;
  stroke-linecap: round;
}

.kitty-whiskers path {
  fill: none;
  stroke: #1f1f2e;
  stroke-width: 3px;
  stroke-linecap: round;
}

.hero-text h1 {
  font-size: clamp(1.9rem, 4vw, 2.6rem);
  margin-bottom: 0.75rem;
  color: #20203a;
}

.hero-text p {
  font-size: 1rem;
  line-height: 1.7;
  margin-bottom: 1.75rem;
}

.main-button {
  background: linear-gradient(135deg, #ff85c0, #ffa4e7);
  border: none;
  color: white;
  padding: 0.85rem 1.6rem;
  font-weight: 700;
  border-radius: 999px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  box-shadow: 0 12px 25px rgba(255, 133, 192, 0.35);
}

.main-button:hover {
  transform: translateY(-3px) scale(1.02);
  box-shadow: 0 18px 35px rgba(255, 133, 192, 0.4);
}

.main-button:focus {
  outline: 3px solid rgba(255, 133, 192, 0.4);
  outline-offset: 2px;
}

.main-button.ghost {
  background: white;
  color: #ff6fc9;
  border: 2px solid rgba(255, 111, 201, 0.35);
  box-shadow: none;
}

.highlight {
  margin-top: 3rem;
  background: rgba(255, 255, 255, 0.85);
  border-radius: 28px;
  padding: clamp(1.5rem, 3vw, 2.5rem);
  box-shadow: 0 16px 35px rgba(93, 112, 190, 0.12);
  border: 2px dashed rgba(31, 31, 46, 0.12);
}

.highlight h2 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
}

.highlight-text {
  font-size: clamp(1.3rem, 3vw, 1.8rem);
  font-weight: 700;
  margin: 0 0 1.5rem;
  color: #191933;
}

.mood-control {
  display: grid;
  gap: 0.75rem;
}

.slider {
  width: 100%;
  accent-color: #ff85c0;
}

.mood-caption {
  font-size: 0.98rem;
  background: rgba(255, 178, 222, 0.18);
  border-radius: 18px;
  padding: 0.75rem 1rem;
  border: 1px solid rgba(255, 178, 222, 0.45);
}

.missions {
  margin-top: 3rem;
  background: rgba(255, 255, 255, 0.92);
  padding: clamp(1.5rem, 3vw, 2.5rem);
  border-radius: 28px;
  box-shadow: 0 16px 30px rgba(68, 71, 120, 0.1);
}

.missions-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 1rem;
}

.progress-pill {
  background: rgba(31, 31, 46, 0.85);
  color: white;
  padding: 0.35rem 1rem;
  border-radius: 999px;
  font-size: 0.85rem;
  letter-spacing: 0.02em;
}

.missions ul {
  list-style: none;
  margin: 1.5rem 0 0;
  padding: 0;
  display: grid;
  gap: 1rem;
}

.mission-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.95), rgba(245, 248, 255, 0.95));
  border-radius: 24px;
  padding: 1.25rem 1.4rem;
  border: 2px solid rgba(31, 31, 46, 0.08);
  display: grid;
  gap: 0.6rem;
  transition: transform 0.2s ease, border 0.2s ease, box-shadow 0.2s ease;
}

.mission-card header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.mission-card.done {
  border-color: rgba(255, 133, 192, 0.55);
  box-shadow: 0 16px 25px rgba(255, 133, 192, 0.18);
  transform: translateY(-4px);
}

.mission-card h3 {
  margin: 0;
  font-size: 1.1rem;
}

.mission-card p {
  margin: 0;
  line-height: 1.6;
}

.mini-button {
  border: none;
  background: rgba(255, 133, 192, 0.16);
  color: #ff3d9a;
  padding: 0.45rem 1rem;
  border-radius: 999px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.2s ease;
}

.mini-button:hover {
  background: rgba(255, 133, 192, 0.32);
  transform: translateY(-2px);
}

.playlist {
  margin-top: 3rem;
  background: rgba(255, 255, 255, 0.88);
  border-radius: 28px;
  padding: clamp(1.5rem, 3vw, 2.4rem);
  border: 2px solid rgba(31, 31, 46, 0.08);
  display: grid;
  gap: 1rem;
}

.options {
  display: grid;
  gap: 0.8rem;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
}

.sound-option {
  background: rgba(255, 230, 246, 0.6);
  border-radius: 20px;
  padding: 0.85rem 1rem;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  border: 1px solid rgba(255, 133, 192, 0.25);
  cursor: pointer;
}

.sound-option input {
  accent-color: #1f1f2e;
}

.playlist-note {
  font-size: 0.92rem;
  color: #575777;
}

.memory {
  margin-top: 3rem;
  background: rgba(255, 255, 255, 0.92);
  padding: clamp(1.5rem, 3vw, 2.5rem);
  border-radius: 28px;
  border: 2px dashed rgba(31, 31, 46, 0.1);
}

.memory ol {
  margin: 0;
  padding-left: 1.4rem;
  display: grid;
  gap: 0.8rem;
}

.memory time {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 700;
  color: #ff5fb6;
  margin-right: 0.6rem;
  letter-spacing: 0.04em;
}

.memory span {
  background: rgba(243, 247, 255, 0.8);
  padding: 0.4rem 0.65rem;
  border-radius: 14px;
  display: inline-block;
}

.footer {
  margin-top: 3.5rem;
  text-align: center;
  color: #494969;
  font-size: 0.95rem;
}

.footer p {
  margin-bottom: 0.6rem;
}

.footer-kitty {
  margin-left: 0.4rem;
  font-size: 1.2rem;
}

.floating-hearts {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.heart {
  position: absolute;
  bottom: -10%;
  animation: float-heart 3.6s ease-in forwards;
  color: rgba(255, 133, 192, 0.85);
}

@keyframes float-heart {
  0% {
    transform: translateY(0) scale(0.85);
    opacity: 0;
  }
  15% {
    opacity: 1;
  }
  70% {
    opacity: 1;
  }
  100% {
    transform: translateY(-120%) scale(1.6);
    opacity: 0;
  }
}

.sparkle-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.sparkle {
  position: absolute;
  bottom: 12%;
  width: 16px;
  height: 16px;
  background: radial-gradient(circle, #fff 0%, #fff 40%, rgba(255, 133, 192, 0) 70%);
  animation: sparkle-pop linear forwards;
  opacity: 0.9;
}

@keyframes sparkle-pop {
  0% {
    transform: translateY(0) scale(0.4) rotate(0deg);
    opacity: 0;
  }
  30% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110px) scale(1.2) rotate(180deg);
    opacity: 0;
  }
}

@media (max-width: 860px) {
  .hero {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .missions-header {
    justify-content: center;
  }
}
</style>
