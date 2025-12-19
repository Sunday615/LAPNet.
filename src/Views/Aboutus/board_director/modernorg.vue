<template>
  <section ref="root" class="hero">
    <!-- Premium tech background -->
    <div class="bg" aria-hidden="true">
      <div ref="orbA" class="orb orb-a"></div>
      <div ref="orbB" class="orb orb-b"></div>
      <div class="grid"></div>
      <div class="vignette"></div>
    </div>

    <div class="container">
      <header class="head">
        <p class="kicker">Organization</p>
        <h1 class="title">Org Chart</h1>
        <p class="subtitle">Elegant premium cards with modern GSAP motion (no lines).</p>
      </header>

      <div ref="chart" class="chart" role="list" aria-label="Organization chart">
        <!-- Row 1 -->
        <div class="row row-1" role="listitem">
          <article
            class="card"
            :ref="setCardRef"
            @mouseenter="hoverIn($event.currentTarget)"
            @mouseleave="hoverOut($event.currentTarget)"
          >
            <div class="card-top">
              <img class="avatar" :src="president.img" :alt="president.name" />
              <div class="meta">
                <div class="name">{{ president.name }}</div>
                <div class="role">{{ president.role }}</div>
                <div class="bank">Bank: {{ president.bank }}</div>
              </div>
            </div>
          </article>
        </div>

        <!-- Row 2 -->
        <div class="row row-2" role="listitem">
          <article
            class="card"
            :ref="setCardRef"
            @mouseenter="hoverIn($event.currentTarget)"
            @mouseleave="hoverOut($event.currentTarget)"
          >
            <div class="card-top">
              <img class="avatar" :src="vicePresident.img" :alt="vicePresident.name" />
              <div class="meta">
                <div class="name">{{ vicePresident.name }}</div>
                <div class="role">{{ vicePresident.role }}</div>
                <div class="bank">Bank: {{ vicePresident.bank }}</div>
              </div>
            </div>
          </article>
        </div>

        <!-- Row 3 -->
        <div class="row row-3" role="listitem">
          <article
            v-for="p in membersRow3"
            :key="p.id"
            class="card"
            :ref="setCardRef"
            @mouseenter="hoverIn($event.currentTarget)"
            @mouseleave="hoverOut($event.currentTarget)"
          >
            <div class="card-top">
              <img class="avatar" :src="p.img" :alt="p.name" />
              <div class="meta">
                <div class="name">{{ p.name }}</div>
                <div class="role">{{ p.role }}</div>
                <div class="bank">Bank: {{ p.bank }}</div>
              </div>
            </div>
          </article>
        </div>

        <!-- Row 4 -->
        <div class="row row-4" role="listitem">
          <article
            v-for="p in membersRow4"
            :key="p.id"
            class="card"
            :ref="setCardRef"
            @mouseenter="hoverIn($event.currentTarget)"
            @mouseleave="hoverOut($event.currentTarget)"
          >
            <div class="card-top">
              <img class="avatar" :src="p.img" :alt="p.name" />
              <div class="meta">
                <div class="name">{{ p.name }}</div>
                <div class="role">{{ p.role }}</div>
                <div class="bank">Bank: {{ p.bank }}</div>
              </div>
            </div>
          </article>
        </div>
      </div>
      <!-- /chart -->
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, onBeforeUpdate } from "vue";
import gsap from "gsap";

const root = ref(null);
const chart = ref(null);
const orbA = ref(null);
const orbB = ref(null);

const cardRefs = ref([]);

onBeforeUpdate(() => {
  cardRefs.value = [];
});

const setCardRef = (el) => el && cardRefs.value.push(el);

// Self-contained avatar (SVG data URI)
function avatar(name) {
  const initials = name
    .split(" ")
    .filter(Boolean)
    .slice(0, 2)
    .map((w) => w[0]?.toUpperCase())
    .join("");

  const svg = `
  <svg xmlns="http://www.w3.org/2000/svg" width="180" height="180" viewBox="0 0 180 180">
    <defs>
      <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
        <stop offset="0" stop-color="#6ee7ff"/>
        <stop offset="0.5" stop-color="#2f7cff"/>
        <stop offset="1" stop-color="#7c3aed"/>
      </linearGradient>
      <radialGradient id="r" cx="35%" cy="30%" r="75%">
        <stop offset="0" stop-color="rgba(255,255,255,0.35)"/>
        <stop offset="1" stop-color="rgba(255,255,255,0)"/>
      </radialGradient>
    </defs>
    <rect width="180" height="180" rx="36" fill="url(#g)"/>
    <rect width="180" height="180" rx="36" fill="url(#r)"/>
    <text x="50%" y="54%" text-anchor="middle" dominant-baseline="middle"
      font-family="ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto"
      font-size="58" font-weight="800" fill="rgba(5,11,26,0.85)">${initials}</text>
  </svg>`;
  return `data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg.trim())}`;
}

// DATA (edit freely)
const president = {
  id: "p",
  name: "Ariana Chen",
  role: "President",
  bank: "NovaBank",
  img: avatar("Ariana Chen"),
};

const vicePresident = {
  id: "vp",
  name: "Daniel Park",
  role: "Vice President",
  bank: "NovaBank",
  img: avatar("Daniel Park"),
};

const membersRow3 = [
  { id: "m1", name: "Mina Sato", role: "Member", bank: "NovaBank", img: avatar("Mina Sato") },
  { id: "m2", name: "Noah Kim", role: "Member", bank: "NovaBank", img: avatar("Noah Kim") },
  { id: "m3", name: "Sofia Patel", role: "Member", bank: "NovaBank", img: avatar("Sofia Patel") },
];

const membersRow4 = [
  { id: "m4", name: "Ethan Brooks", role: "Member", bank: "NovaBank", img: avatar("Ethan Brooks") },
  { id: "m5", name: "Linh Tran", role: "Member", bank: "NovaBank", img: avatar("Linh Tran") },
  { id: "m6", name: "Amir Khan", role: "Member", bank: "NovaBank", img: avatar("Amir Khan") },
  { id: "m7", name: "Chloe Martin", role: "Member", bank: "NovaBank", img: avatar("Chloe Martin") },
];

function hoverIn(el) {
  gsap.to(el, { scale: 1.03, duration: 0.22, ease: "power2.out" });
}
function hoverOut(el) {
  gsap.to(el, { scale: 1, duration: 0.28, ease: "power2.out" });
}

let ctx;
onMounted(() => {
  const reduceMotion = window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches;

  ctx = gsap.context(() => {
    // Entrance
    gsap.set(cardRefs.value, {
      opacity: 0,
      y: 22,
      rotateX: 10,
      transformPerspective: 900,
      transformOrigin: "center",
    });

    gsap.to(cardRefs.value, {
      opacity: 1,
      y: 0,
      rotateX: 0,
      duration: 0.9,
      ease: "power3.out",
      stagger: 0.07,
    });

    // Background orbs
    if (!reduceMotion) {
      gsap.to(orbA.value, { x: 70, y: -45, duration: 10, ease: "sine.inOut", yoyo: true, repeat: -1 });
      gsap.to(orbB.value, { x: -60, y: 55, duration: 12, ease: "sine.inOut", yoyo: true, repeat: -1 });
    }

    // Subtle float
    if (!reduceMotion) {
      gsap.to(cardRefs.value, {
        y: "-=6",
        duration: 3.4,
        ease: "sine.inOut",
        yoyo: true,
        repeat: -1,
        stagger: { each: 0.12, from: "random" },
      });
    }
  }, root.value);
});

onUnmounted(() => ctx?.revert());
</script>

<style scoped>
.hero {
  position: relative;
  overflow: hidden;
  padding: clamp(32px, 6vw, 72px) 0;
  color: #e9f2ff;
  background: linear-gradient(180deg, #050b1a 0%, #071833 55%, #061226 100%);
}

.bg {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.orb {
  position: absolute;
  width: 520px;
  height: 520px;
  filter: blur(42px);
  opacity: 0.55;
  border-radius: 999px;
  background: radial-gradient(circle at 30% 30%, rgba(110, 231, 255, 0.9), rgba(47, 124, 255, 0.3), rgba(124, 58, 237, 0.15));
  mix-blend-mode: screen;
}
.orb-a { top: -180px; left: -180px; }
.orb-b { bottom: -220px; right: -220px; opacity: 0.45; }

.grid {
  position: absolute;
  inset: 0;
  opacity: 0.18;
  background:
    linear-gradient(to right, rgba(255,255,255,0.08) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255,255,255,0.06) 1px, transparent 1px);
  background-size: 48px 48px;
  mask-image: radial-gradient(circle at 50% 25%, rgba(0,0,0,1), rgba(0,0,0,0));
}

.vignette {
  position: absolute;
  inset: -2px;
  background: radial-gradient(circle at 50% 20%, rgba(0,0,0,0), rgba(0,0,0,0.55) 70%, rgba(0,0,0,0.75));
}

.container {
  position: relative;
  width: min(1100px, calc(100% - 40px));
  margin: 0 auto;
}

.head {
  margin-bottom: clamp(20px, 4vw, 34px);
  text-align: center;
}

.kicker {
  letter-spacing: 0.16em;
  text-transform: uppercase;
  font-size: 12px;
  color: rgba(233, 242, 255, 0.72);
}

.title {
  margin: 10px 0 8px;
  font-size: clamp(30px, 4.3vw, 50px);
  line-height: 1.05;
  font-weight: 800;
  background: linear-gradient(90deg, rgba(233,242,255,1), rgba(110,231,255,0.95), rgba(47,124,255,0.95));
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.subtitle {
  margin: 0;
  color: rgba(233, 242, 255, 0.7);
  font-size: 14px;
}

.chart {
  display: grid;
  gap: 18px;
  padding-top: 6px;
}

.row {
  display: grid;
  gap: 14px;
}

.row-1, .row-2 {
  place-items: center;
}

.row-3 {
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.row-4 {
  grid-template-columns: repeat(4, minmax(0, 1fr));
}

@media (max-width: 920px) {
  .row-4 { grid-template-columns: repeat(2, minmax(0, 1fr)); }
}
@media (max-width: 720px) {
  .row-3 { grid-template-columns: repeat(1, minmax(0, 1fr)); }
  .row-4 { grid-template-columns: repeat(1, minmax(0, 1fr)); }
}

.card {
  position: relative;
  width: 100%;
  max-width: 560px;
  border-radius: 24px;
  padding: 18px 18px;
  background: linear-gradient(180deg, rgba(10, 28, 64, 0.75), rgba(6, 16, 38, 0.6));
  border: 1px solid rgba(110, 231, 255, 0.14);
  box-shadow:
    0 18px 44px rgba(0, 0, 0, 0.42),
    inset 0 1px 0 rgba(255, 255, 255, 0.06);
  backdrop-filter: blur(10px);
  transform-style: preserve-3d;
  transition: border-color 220ms ease, box-shadow 220ms ease;
}

.card:hover {
  border-color: rgba(110, 231, 255, 0.32);
  box-shadow:
    0 22px 62px rgba(0, 0, 0, 0.55),
    0 0 0 1px rgba(110, 231, 255, 0.08) inset;
}

.card-top {
  display: flex;
  align-items: center;
  gap: 14px;
}

/* BIGGER profile */
.avatar {
  width: 78px;
  height: 78px;
  border-radius: 22px;
  object-fit: cover;
  box-shadow: 0 14px 30px rgba(0,0,0,0.38);
  border: 1px solid rgba(255,255,255,0.14);
}

.meta .name {
  font-weight: 800;
  letter-spacing: 0.2px;
  font-size: 16px;
}

.meta .role {
  margin-top: 3px;
  font-size: 13px;
  color: rgba(233, 242, 255, 0.76);
}

.meta .bank {
  margin-top: 7px;
  font-size: 12px;
  color: rgba(110, 231, 255, 0.78);
}
</style>
