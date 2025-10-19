<script setup lang="ts">
const sections = [
  {
    id: 'meow-halo',
    title: 'Halaman Halo',
    cat: 'ฅ^•ﻌ•^ฅ',
    highlight: 'Hai, Paw!',
    note: 'Mulai dengan senyum lucu.',
    accent: '#ff9ac2',
    extra: 'sparkles',
  },
  {
    id: 'meow-regang',
    title: 'Halaman Regang',
    cat: '(=^･ω･^=)',
    highlight: 'Geliat Dulu',
    note: 'Gerak lembut dan lentur.',
    accent: '#ffd84d',
    extra: 'stretch',
  },
  {
    id: 'meow-mini',
    title: 'Halaman Mini',
    cat: '(=｀ω´=)',
    highlight: 'Mini Target',
    note: 'Langkah imut cukup.',
    accent: '#b4f0ff',
    extra: 'goals',
  },
  {
    id: 'meow-dance',
    title: 'Halaman Goyang',
    cat: '(=^ ◡ ^=)',
    highlight: 'Goyang Yuk',
    note: 'Ikuti beat manis.',
    accent: '#c4ffbb',
    extra: 'beats',
  },
  {
    id: 'meow-magic',
    title: 'Halaman Mantra',
    cat: '(=^・ω・^=)',
    highlight: 'Mantra Meong',
    note: 'Bisik semangat.',
    accent: '#dabfff',
    extra: 'mantras',
  },
  {
    id: 'meow-relax',
    title: 'Halaman Rehat',
    cat: '(=^‥^=)',
    highlight: 'Rehat Hangat',
    note: 'Tarik napas lagi.',
    accent: '#ffc6ff',
    extra: 'breaks',
  },
]

const sparkleBubbles = ['Senyum', 'Kedip', 'Peluk', 'Yeay', 'Nyan'];

const stretchMoves = [
  { emoji: '↺', text: 'Putar Bahu' },
  { emoji: '🐾', text: 'Cakar Imut' },
  { emoji: '🙆‍♀️', text: 'Lentur Lengan' },
]

const microGoals = [
  { icon: '📝', text: 'Catat 1 hal' },
  { icon: '📦', text: 'Rapikan pojok' },
  { icon: '🍪', text: 'Snack hadiah' },
]

const danceBeats = ['Head bop', 'Snap paw', 'Spin mini', 'Wave tail'];

const sweetMantras = ['Aku kuat', 'Aku lucu', 'Aku meong', 'Aku siap'];

const cozyBreaks = ['Napas 4x', 'Minum hangat', 'Tutup mata', 'Stretch lagi'];

const catGallery = [
  {
    ascii: ` /\_/\\
( o.o )
 > ^ < `,
    caption: 'Miu: meong ceria!',
  },
  {
    ascii: ` ／l、
（ﾟ､ ｡ ７
 l  ~ヽ
 じしf_, )ノ`,
    caption: 'Paw: siap sorak!',
  },
  {
    ascii: `|\---/|
| o_o |
 \_^_/`,
    caption: 'Tux: fokus lembut.',
  },
  {
    ascii: ` /\_/\\
(=^‥^=)
(")_(")`,
    caption: 'Lulu: peluk dulu.',
  },
]
</script>

<template>
  <div class="app">
    <header class="hero" id="meow-halo">
      <nav class="nav">
        <a v-for="section in sections" :key="section.id" class="nav__item" :href="`#${section.id}`">
          {{ section.title }}
        </a>
      </nav>
      <div class="hero__content">
        <div class="hero__cat" aria-hidden="true">ฅ^•ﻌ•^ฅ</div>
        <h1>Meow Boost</h1>
        <p class="hero__tagline">Lucu · Imut · Semangat</p>
        <div class="hero__cta">
          <a class="button" href="#meow-regang">Mulai 🐾</a>
          <a class="button button--ghost" href="#galeri">Lihat Galeri</a>
        </div>
      </div>
      <div class="hero__paws">
        <span v-for="index in 16" :key="index">🐾</span>
      </div>
    </header>

    <main>
      <section
        v-for="section in sections"
        :key="section.id"
        :id="section.id"
        class="section"
        :style="{ '--accent': section.accent }"
      >
        <div class="section__card">
          <div class="section__cat" aria-hidden="true">{{ section.cat }}</div>
          <h2>{{ section.highlight }}</h2>
          <p>{{ section.note }}</p>
        </div>

        <div v-if="section.extra === 'sparkles'" class="section__extra section__extra--chips">
          <span v-for="bubble in sparkleBubbles" :key="bubble" class="chip">{{ bubble }}</span>
        </div>

        <div v-else-if="section.extra === 'stretch'" class="section__extra section__extra--list">
          <article v-for="move in stretchMoves" :key="move.text" class="card">
            <div class="card__icon">{{ move.emoji }}</div>
            <p>{{ move.text }}</p>
          </article>
        </div>

        <div v-else-if="section.extra === 'goals'" class="section__extra section__extra--list">
          <article v-for="goal in microGoals" :key="goal.text" class="card">
            <div class="card__icon">{{ goal.icon }}</div>
            <p>{{ goal.text }}</p>
          </article>
        </div>

        <div v-else-if="section.extra === 'beats'" class="section__extra section__extra--chips">
          <span v-for="beat in danceBeats" :key="beat" class="chip chip--beat">{{ beat }}</span>
        </div>

        <div v-else-if="section.extra === 'mantras'" class="section__extra section__extra--list">
          <article v-for="mantra in sweetMantras" :key="mantra" class="card card--soft">
            <p>{{ mantra }}</p>
          </article>
        </div>

        <div v-else class="section__extra section__extra--chips">
          <span v-for="idea in cozyBreaks" :key="idea" class="chip chip--calm">{{ idea }}</span>
        </div>
      </section>

      <section class="gallery" id="galeri" aria-label="Galeri Kucing Ilustrasi">
        <h2>Galeri Meow</h2>
        <p>Pilih maskot favoritmu.</p>
        <div class="gallery__grid">
          <figure v-for="cat in catGallery" :key="cat.caption" class="gallery__item">
            <pre aria-hidden="true">{{ cat.ascii }}</pre>
            <figcaption>{{ cat.caption }}</figcaption>
          </figure>
        </div>
      </section>
    </main>

    <footer class="footer">
      <p>Meong hugs dikirim setiap detik ♡</p>
    </footer>
  </div>
</template>

<style scoped>
:global(body) {
  margin: 0;
  font-family: 'Nunito', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: radial-gradient(circle at top, #fff6fb 0%, #fff 50%, #ffe9fb 100%);
  color: #2e1b36;
  scroll-behavior: smooth;
}

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.hero {
  position: relative;
  padding: 4rem 1.5rem 5rem;
  background: linear-gradient(145deg, #ffdff6, #ffeefc 55%, #f3f6ff);
  text-align: center;
  overflow: hidden;
}

.nav {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.nav__item {
  padding: 0.5rem 1.1rem;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.8);
  color: #a62468;
  font-weight: 700;
  text-decoration: none;
  font-size: 0.9rem;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.nav__item:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(166, 36, 104, 0.15);
}

.hero__content {
  max-width: 420px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.hero__cat {
  font-size: clamp(2.5rem, 10vw, 4rem);
  animation: floaty 3.5s ease-in-out infinite;
}

.hero h1 {
  font-size: clamp(2.4rem, 7vw, 3.2rem);
  color: #7c2c77;
  margin: 0;
}

.hero__tagline {
  margin: 0;
  font-size: 1.1rem;
  color: #a8549b;
  letter-spacing: 0.08em;
}

.hero__cta {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
}

.button {
  display: inline-block;
  padding: 0.65rem 1.4rem;
  border-radius: 999px;
  font-weight: 700;
  background: linear-gradient(135deg, #ff8fc7, #ffb3d9);
  color: #fff;
  text-decoration: none;
  box-shadow: 0 12px 25px rgba(255, 143, 199, 0.35);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.button:hover {
  transform: translateY(-3px);
  box-shadow: 0 16px 28px rgba(255, 143, 199, 0.45);
}

.button--ghost {
  background: rgba(255, 255, 255, 0.65);
  color: #a62468;
  box-shadow: 0 0 0 rgba(0, 0, 0, 0);
}

.button--ghost:hover {
  box-shadow: 0 12px 24px rgba(166, 36, 104, 0.12);
}

.hero__paws {
  position: absolute;
  inset: 0;
  pointer-events: none;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  font-size: 1.5rem;
  opacity: 0.15;
  mix-blend-mode: multiply;
  animation: paw-drift 12s linear infinite;
}

.section {
  padding: 4rem 1.5rem;
  display: grid;
  gap: 1.5rem;
  justify-items: center;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.65));
}

.section:nth-child(even) {
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.8), rgba(255, 249, 254, 0.6));
}

.section__card {
  position: relative;
  padding: 2.5rem 2rem 2rem;
  border-radius: 32px;
  background: #fff;
  box-shadow: 0 15px 35px rgba(174, 67, 150, 0.12);
  text-align: center;
  max-width: 420px;
  border: 3px dashed rgba(0, 0, 0, 0.03);
}

.section__card::after {
  content: '';
  position: absolute;
  inset: 8px;
  border-radius: 28px;
  border: 1px solid rgba(0, 0, 0, 0.04);
  pointer-events: none;
}

.section__cat {
  font-size: clamp(2rem, 8vw, 3rem);
  margin-bottom: 1rem;
}

.section__card h2 {
  margin: 0;
  font-size: 1.9rem;
  color: #7c2c77;
}

.section__card p {
  margin: 0.6rem 0 0;
  color: #a8549b;
  font-size: 1rem;
}

.section__extra {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 640px;
}

.section__extra--chips {
  gap: 0.8rem;
}

.chip {
  padding: 0.55rem 1.1rem;
  border-radius: 999px;
  background: color-mix(in srgb, var(--accent, #ffd8ef) 60%, white 40%);
  color: #7c2c77;
  font-weight: 700;
  font-size: 0.95rem;
  box-shadow: 0 12px 25px rgba(124, 44, 119, 0.08);
  letter-spacing: 0.04em;
}

.chip--beat {
  background: color-mix(in srgb, var(--accent, #ffd8ef) 50%, #e8f8ff 50%);
}

.chip--calm {
  background: color-mix(in srgb, var(--accent, #ffd8ef) 40%, #f3ffe7 60%);
}

.section__extra--list {
  gap: 1.2rem;
}

.card {
  width: 150px;
  padding: 1.2rem;
  border-radius: 24px;
  background: color-mix(in srgb, var(--accent, #ffd8ef) 20%, white 80%);
  box-shadow: 0 15px 30px rgba(124, 44, 119, 0.1);
  text-align: center;
  font-weight: 600;
  color: #7c2c77;
}

.card__icon {
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
}

.card--soft {
  width: auto;
  padding: 0.9rem 1.4rem;
}

.gallery {
  padding: 4rem 1.5rem 5rem;
  text-align: center;
  background: linear-gradient(145deg, #fff6fb, #fff0ff 55%, #f6fff9);
}

.gallery h2 {
  margin: 0;
  font-size: 2.1rem;
  color: #7c2c77;
}

.gallery p {
  color: #a8549b;
  margin-bottom: 2rem;
  font-weight: 600;
  letter-spacing: 0.05em;
}

.gallery__grid {
  display: grid;
  gap: 1.5rem;
  justify-content: center;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  max-width: 840px;
  margin: 0 auto;
}

.gallery__item {
  padding: 1.5rem;
  border-radius: 24px;
  background: rgba(255, 255, 255, 0.85);
  box-shadow: 0 12px 25px rgba(174, 67, 150, 0.12);
  color: #7c2c77;
  font-weight: 600;
}

.gallery__item pre {
  font-family: 'Fira Code', 'Courier New', Courier, monospace;
  font-size: 0.85rem;
  margin: 0 0 0.8rem;
  color: #5a2f6b;
}

.footer {
  text-align: center;
  padding: 2.5rem 1rem 3rem;
  font-weight: 600;
  color: #a8549b;
}

@keyframes floaty {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
}

@keyframes paw-drift {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-40px);
  }
}

@media (min-width: 768px) {
  .hero {
    padding: 5rem 2rem 6rem;
  }

  .hero__content {
    max-width: 520px;
  }

  .section {
    padding: 5rem 2rem;
  }

  .section__card {
    max-width: 480px;
  }
}
</style>
