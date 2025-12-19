<template>
  <main_navbar
    title="ສະພາຜູ້ບໍລິຫານ"
    :breadcrumb="['ໜ້າຫຼັກ', 'ກ່ຽວກັບພວກເຮົາ', 'ຄະນະສະພາຜູ້ບໍລິຫານ']"
    background-image="/aboutus/navigatormission-bg.png"
  />
  <div class="navbardirector">
    <bod_navbar />
  </div>

  <div class="chart-page">
    <div class="chart-wrapper">
      <h1 class="chart-title">ຄະນະສະພາຜູ້ບໍລິຫານ</h1>

      <p class="chart-subtitle" style="display:flex ; align-items: center;">
        <img
          src="/logolapnet/fullcircle.png"
          style="width: 25px; height:25px ; margin-right: 5px;"
          alt=""
        />
        ຄະນະບໍລິຫານງານ Lao National Payment Network CO., LTD
      </p>

      <!-- DESKTOP LAYOUT -->
      <div class="chart-grid-desktop">
        <div
          v-for="(row, rowIndex) in cleanLayout"
          :key="'row-' + rowIndex"
          :class="[
            'chart-row',
            rowIndex === cleanLayout.length - 1 ? 'chart-row--grid' : 'chart-row--flex'
          ]"
        >
          <div
            v-for="(slot, colIndex) in row"
            :key="'cell-' + rowIndex + '-' + colIndex"
            class="chart-cell"
            :class="[
              slot.roleTag === 'president' && 'chart-cell--president',
              slot.roleTag === 'vp' && 'chart-cell--vp'
            ]"
          >
            <div
              class="member-card"
              :class="[
                slot.roleTag === 'president' && 'member-card--president',
                slot.roleTag === 'vp' && 'member-card--vp',
                (rowIndex === 0 || rowIndex === 1 || rowIndex === cleanLayout.length - 1) &&
                  'member-card--large'
              ]"
              @mouseenter="onCardEnter"
              @mouseleave="onCardLeave"
            >
              <!-- ✅ light sweep (hover only) -->
              <span class="shine" aria-hidden="true"></span>

              <div class="avatar-wrapper">
                <img class="avatar-image" :src="slot.image" :alt="slot.name" />
              </div>

              <div class="member-info">
                <div class="member-name">{{ slot.name }}</div>
                <div class="member-role">{{ slot.role }}</div>

                <!-- ✅ NEW: modern underline + bank mini logo (left) + bank name (right) -->
                <div v-if="slot.bankName || slot.bankLogo" class="member-bank">
                  <span class="bank-line" aria-hidden="true"></span>

                  <div class="bank-row">
                    <img
                      v-if="slot.bankLogo"
                      class="bank-logo"
                      :src="slot.bankLogo"
                      :alt="slot.bankName || 'bank'"
                    />
                    <span v-if="slot.bankName" class="bank-name">
                      {{ slot.bankName }}
                    </span>
                  </div>
                </div>
                <!-- ✅ END -->
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- MOBILE LAYOUT (President first, VP second) -->
      <div class="chart-grid-mobile">
        <div
          v-for="(member, index) in mobileMembers"
          :key="'mobile-' + index"
          class="chart-cell mobile-cell"
        >
          <div
            class="member-card"
            :class="[
              member.roleTag === 'president' && 'member-card--president',
              member.roleTag === 'vp' && 'member-card--vp'
            ]"
            @mouseenter="onCardEnter"
            @mouseleave="onCardLeave"
          >
            <!-- ✅ light sweep (hover only) -->
            <span class="shine" aria-hidden="true"></span>

            <div class="avatar-wrapper">
              <img class="avatar-image" :src="member.image" :alt="member.name" />
            </div>

            <div class="member-info">
              <div class="member-name">{{ member.name }}</div>
              <div class="member-role">{{ member.role }}</div>

              <div v-if="member.bankName || member.bankLogo" class="member-bank">
                <span class="bank-line" aria-hidden="true"></span>

                <div class="bank-row">
                  <img
                    v-if="member.bankLogo"
                    class="bank-logo"
                    :src="member.bankLogo"
                    :alt="member.bankName || 'bank'"
                  />
                  <span v-if="member.bankName" class="bank-name">
                    {{ member.bankName }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- END MOBILE LAYOUT -->
    </div>
  </div>

  <div class="boxmargin" style="width: 100%; height: 15vh"></div>
  <secondfooter />
</template>

<script setup>
import { onMounted, onBeforeUnmount, nextTick, computed } from "vue";
import { gsap } from "gsap";
import main_navbar from "../../../components/miannavbar/main_navbar.vue";
import bod_navbar from "../companystructure/navbarcompany/bod_navbar.vue";
import secondfooter from "../../../components/footer/mainfooter/secondfooter.vue";

/* =========================
   DATA (เพิ่ม bankName/bankLogo ได้ตามต้องการ)
   ========================= */
const layout = [
  [
    null,
    {
      type: "member",
      name: "ທ່ານ ມະໂນລິດ ຊຸມພົນພັກດີ",
      role: "ປະທານສະພາບໍລິຫານ",
      roleTag: "president",
      image: "/board-director-profile/BOL.webp",
      bankName: "ທະນາຄານແຫ່ງ ສປປ ລາວ (BOL)",
      bankLogo: "/logoallmember/circle_scale/BOL.png", 
    },
    null,
    null,
  ],
  [
    null,
    {
      type: "member",
      name: "ທ່ານ ນັນທະລາດ ແກ້ວປະເສີດ",
      role: "ຮອງປະທານສະພາບໍລິຫານ",
      roleTag: "vp",
      image: "/board-director-profile/allbcel.png",
      bankName: "ທະນາຄານ ການຄ້າຕ່າງປະເທດລາວ ມະຫາຊົນ ",
      bankLogo: "/logoallmember/circle_scale/BCEL.png",
    },
    null,
    null,
  ],
  [
    {
      type: "member",
      name: "ທ່ານ CHU XUEMEI",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/upi.webp",
      bankName: "ບໍລິສັດ ຢູນຽນເພ ສາກົນ ຈຳກັດ ",
      bankLogo: "/logoallmember/circle_scale/UPI.png",
    },
    {
      type: "member",
      name: "ທ່ານ ສອນຕາວັນ ໄກສອນເສນາ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/ldb.webp",
      bankName: "ທະນາຄານ ພັດທະນາລາວ ຈຳກັດ ",
      bankLogo: "/logoallmember/circle_scale/LDB.PNG",
    },
    {
      type: "member",
      name: "ທ່ານ ສີສະອາດ ນຶມອາສາ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/apbcircle.png",
      bankName: "ທະນາຄານ ສົ່ງເສີມກະສິກຳ ຈຳກັດ ",
      bankLogo: "/logoallmember/circle_scale/APBB.PNG",
    },
    null,
  ],
  [
    {
      type: "member",
      name: "ທ່ານ ວຽງວິໄລ ແສງຄຳຢອງ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/lvb.png",
      bankName: "ທະນາຄານ ຮ່ວມທຸລະກິດລາວ-ຫວຽດ ",
      bankLogo: "/logoallmember/circle_scale/lvb.PNG",
    },
    {
      type: "member",
      name: "ທ່ານ ຈັນຊະນະ ສິງຫາວົງ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/alljdb.png",
      bankName: "ທະນາຄານ ຮ່ວມພັດທະນາ ມະຫາຊົນ ",
      bankLogo: "/logoallmember/circle_scale/JDB.png",
    },
    {
      type: "member",
      name: "ທ່ານ ເອກະລາດ ລັດຕະນະຈານ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/logoallmember/circle_scale/STB.png",
      bankName: "ທະນາຄານ ເອັສທີ ຈຳກັດ ",
      bankLogo: "/logoallmember/circle_scale/STB.png",
    },
    {
      type: "member",
      name: "ທ່ານ ຟີລິກ ດີຟຣານຊິສ",
      role: "ສະມາຊິກສະພາບໍລິຫານ",
      roleTag: "member",
      image: "/board-director-profile/bic.png",
      bankName: "ທະນາຄານ ບີໄອຊີ ລາວ ຈຳກັດ ",
      bankLogo: "/logoallmember/circle_scale/BIC.png",
    },
  ],
];

const cleanLayout = computed(() =>
  layout.map((row) => row.filter((slot) => slot && slot.type === "member"))
);

const mobileMembers = computed(() => {
  const all = [];
  layout.forEach((row) => row.forEach((slot) => slot && slot.type === "member" && all.push(slot)));

  const presidentIndex = all.findIndex((m) => m.roleTag === "president");
  const vpIndex = all.findIndex((m) => m.roleTag === "vp");

  const result = [];
  if (presidentIndex !== -1) result.push(all[presidentIndex]);
  if (vpIndex !== -1) result.push(all[vpIndex]);

  all.forEach((m, idx) => idx !== presidentIndex && idx !== vpIndex && result.push(m));
  return result;
});

/* =========================
   HOVER LIGHT SWEEP (GSAP) — hover only
   ========================= */
const sweepMap = new WeakMap();

function prefersReducedMotion() {
  return (
    typeof window !== "undefined" &&
    window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches
  );
}

function getShine(cardEl) {
  return cardEl?.querySelector?.(".shine") || null;
}

function onCardEnter(e) {
  if (prefersReducedMotion()) return;

  const card = e.currentTarget;
  const shine = getShine(card);
  if (!shine) return;

  // kill previous animation for this card
  const prev = sweepMap.get(card);
  if (prev) {
    prev.kill();
    sweepMap.delete(card);
  }

  // premium smooth sweep: fade in -> cross -> fade out
  gsap.set(shine, { xPercent: -180, opacity: 0 });

  const tl = gsap.timeline();
  tl.to(shine, { opacity: 0.75, duration: 0.18, ease: "power2.out" }, 0);
  tl.to(
    shine,
    {
      xPercent: 180,
      duration: 1.25,
      ease: "sine.inOut",
    },
    0
  );
  tl.to(shine, { opacity: 0, duration: 0.38, ease: "power2.out" }, 0.92);

  sweepMap.set(card, tl);
}

function onCardLeave(e) {
  const card = e.currentTarget;
  const shine = getShine(card);
  if (!shine) return;

  const prev = sweepMap.get(card);
  if (prev) {
    prev.kill();
    sweepMap.delete(card);
  }

  // quick clean reset (no flashing)
  gsap.to(shine, {
    opacity: 0,
    duration: 0.14,
    ease: "power2.out",
    onComplete: () => gsap.set(shine, { xPercent: -180 }),
  });
}

onMounted(async () => {
  await nextTick();
  // กัน flash แรกเข้า (ตั้งค่าเริ่มต้นให้ทุก shine)
  gsap.set(".member-card .shine", { xPercent: -180, opacity: 0 });
});

onBeforeUnmount(() => {
  // kill any remaining timelines
  sweepMap.forEach?.((tl) => tl?.kill?.()); // safe (บาง env ไม่มี forEach บน WeakMap)
});
</script>

<style scoped>
.navbardirector {
  width: 100%;
  height: 20vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.chart-page {
  min-height: 100vh;
  padding: 56px 16px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", sans-serif;
  color: #f5f7ff;
  overflow-x: hidden;
}

/* main card wrapper */
.chart-wrapper {
  width: 100%;
  max-width: 1600px;
  margin: 0 auto;
  padding: 32px 36px 40px;
  box-sizing: border-box;
  border-radius: 32px;
  background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.14), transparent 55%),
    rgba(4, 8, 36, 0.96);
  border: 1px solid rgba(255, 255, 255, 0.16);
  box-shadow: 0 26px 70px rgba(0, 0, 0, 0.65);
  backdrop-filter: blur(20px);
}

.chart-title {
  font-size: clamp(2.2rem, 2.5vw + 1.4rem, 3.5rem);
  font-weight: 700;
  margin: 0 0 6px;
}

.chart-subtitle {
  margin: 0 0 28px;
  font-size: 14px;
  opacity: 0.85;
}

/* DESKTOP GRID WRAPPER */
.chart-grid-desktop {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* Base row (animation + spacing only) */
.chart-row {
  gap: 22px;
  opacity: 0;
  transform: translateY(18px) scale(0.98);
  animation: rowIn 0.7s ease-out forwards;
}

/* last row: 4-column full-width grid */
.chart-row--grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
}

/* row 1–3: centered flex rows */
.chart-row--flex {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.chart-cell {
  display: flex;
  align-items: stretch;
  justify-content: center;
  min-height: 170px;
  flex: 0 1 400px;
  max-width: 100%;
}

/* wider VP */
.chart-row--flex .chart-cell--vp {
  flex: 0 1 480px;
}

/* widest President */
.chart-row--flex .chart-cell--president {
  flex: 0 1 500px;
}

/* MOBILE LIST (hidden by default) */
.chart-grid-mobile {
  display: none;
}

/* Member Card */
.member-card {
  position: relative;
  width: 100%;
  display: flex;
  
  align-items: center;
  gap: 18px;
  padding: 20px 22px;
  border-radius: 24px;
  background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.16), transparent 55%),
    rgba(6, 12, 52, 0.98);
  box-shadow: 0 20px 52px rgba(0, 0, 0, 0.55);
  border: 1px solid rgba(255, 255, 255, 0.12);
  overflow: hidden;
  transition: transform 220ms ease, box-shadow 220ms ease, background 220ms ease;
}

/* Bigger cards: row 1, row 2, row 4 */
.member-card--large {
  padding: 26px 26px;
}

.member-card--large .avatar-wrapper {
  width: 100px;
  height: 100px;
}

.member-card--large .member-name {
  font-size: 18px;
}

.member-card--large .member-role {
  font-size: 1.05rem;
}

/* Glow border effect */
.member-card::before {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1px;
  background: linear-gradient(
    95deg,
    rgba(0, 3, 41, 1) 0%,
    rgba(0, 51, 171, 1) 46%,
    rgba(157, 214, 255, 0.9) 100%
  );
  -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  opacity: 0.7;
  pointer-events: none;
}

/* ✅ Premium light sweep (GSAP animates xPercent + opacity) */
.member-card .shine {
  position: absolute;
  top: -85%;
  left: -55%;
  width: 78%;
  height: 280%;
  pointer-events: none;
  opacity: 0;

  /* prettier highlight (soft edges + bright core) */
  background: linear-gradient(
    120deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.06) 28%,
    rgba(255, 255, 255, 0.42) 50%,
    rgba(255, 255, 255, 0.06) 72%,
    rgba(255, 255, 255, 0) 100%
  );

  filter: blur(8px);
  mix-blend-mode: screen;
  transform: skewX(-18deg);
  will-change: transform, opacity;
}

/* Hover interaction */
.member-card:hover {
  transform: translateY(-8px) scale(1.035);
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.75);
}

.member-card--president {
  background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.2), transparent 55%),
    rgba(8, 20, 90, 0.98);
}

.member-card--vp {
  background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.18), transparent 55%),
    rgba(6, 16, 70, 0.98);
}

.avatar-wrapper {
  flex-shrink: 0;
  width: 100px;
  height: 100px;
  border-radius: 999px;
  padding: 4px;
  background: linear-gradient(135deg, rgba(157, 214, 255, 1), rgba(0, 51, 171, 1));
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar-image {
  width: 100%;
  height: 100%;
  border-radius: inherit;
  object-fit: cover;
  background: #020617;
}

.member-info {
  display: flex;
  flex-direction: column;
  gap: 6px;
  min-width: 0;
}

.member-name {
  font-size: 17px;
  font-weight: 600;
  line-height: 1.25;
  word-break: break-word;
}

.member-role {
  font-size: 1rem;
  text-transform: uppercase;
  opacity: 0.85;
  line-height: 1.25;
}

/* ✅ NEW: Bank underline + row */
.member-bank {
  margin-top: 10px;
  display: grid;
  gap: 10px;
}

.bank-line {
  height: 1px;
  width: 100%;
  border-radius: 999px;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(157, 214, 255, 0.65) 18%,
    rgba(0, 120, 255, 0.85) 50%,
    rgba(157, 214, 255, 0.55) 82%,
    rgba(255, 255, 255, 0) 100%
  );
  box-shadow: 0 0 14px rgba(56, 189, 248, 0.25), 0 0 26px rgba(37, 99, 235, 0.18);
  opacity: 0.95;
  position: relative;
  overflow: hidden;
}

.bank-line::after {
  content: "";
  position: absolute;
  top: -2px;
  left: -30%;
  width: 30%;
  height: 5px;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.45), transparent);
  filter: blur(2px);
  opacity: 0.55;
  animation: lineSweep 2.8s ease-in-out infinite;
}

@keyframes lineSweep {
  0% { transform: translateX(0); }
  100% { transform: translateX(420%); }
}

.bank-row {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.bank-logo {
  width: 30px;
  height: 30px;
  border-radius: 999px;
  object-fit: cover;
  flex-shrink: 0;
  background: rgba(2, 6, 23, 0.7);
  border: 1px solid rgba(255, 255, 255, 0.14);
  box-shadow: 0 0 14px rgba(56, 189, 248, 0.22), 0 0 26px rgba(37, 99, 235, 0.16);
}

.bank-name {
  font-size: 0.86rem;
  font-weight: 600;
  letter-spacing: 0.02em;
  color: rgba(226, 232, 240, 0.92);

  overflow: hidden;
  text-overflow: ellipsis;
}

/* Animations */
@keyframes rowIn {
  0% { opacity: 0; transform: translateY(18px) scale(0.98); }
  100% { opacity: 1; transform: translateY(0) scale(1); }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  .chart-row { animation: none; opacity: 1; transform: none; }
  .member-card { transition: none; }
  .bank-line::after { animation: none; }
}

/* MEDIUM SCREENS – tablets, small laptops */
@media (max-width: 1200px) {
  .chart-wrapper { padding: 28px 22px 32px; }

  .chart-row--flex .chart-cell,
  .chart-row--flex .chart-cell--vp,
  .chart-row--flex .chart-cell--president {
    flex: 1 1 100%;
    max-width: 600px;
  }

  .chart-row--grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }

  .member-card--large { padding: 22px 22px; }
  .member-card--large .avatar-wrapper { width: 100px; height: 100px; }
}

/* RESPONSIVE – mobile (1 column) */
@media (max-width: 900px) {
  .chart-page { padding: 40px 12px; }

  .chart-wrapper {
    padding: 22px 14px 26px;
    max-width: 100%;
    border-radius: 26px;
  }

  .chart-title { text-align: center; }
  .chart-subtitle { text-align: center; justify-content: center; gap: 6px; }

  .chart-grid-desktop { display: none; }
  .chart-grid-mobile {
    display: flex;
    flex-direction: column;
    gap: 14px;
    margin-top: 18px;
  }

  .chart-grid-mobile .chart-cell {
    width: 100%;
    max-width: 100%;
    min-height: auto;
    flex: initial;
    justify-content: stretch;
    align-items: stretch;
  }

  .member-card {
    padding: 14px 14px;
    gap: 12px;
    border-radius: 18px;
    box-shadow: 0 14px 34px rgba(0, 0, 0, 0.55);
  }

  .avatar-wrapper { width: 60px; height: 60px; padding: 3px; }
  .member-info { gap: 4px; }
  .member-name { font-size: 15px; }
  .member-role { font-size: 0.88rem; }

  .member-card:hover {
    transform: translateY(-2px) scale(1.01);
    box-shadow: 0 18px 44px rgba(0, 0, 0, 0.65);
  }

  .member-bank { gap: 8px; margin-top: 8px; }
  .bank-logo { width: 20px; height: 20px; }
  .bank-name { font-size: 0.82rem; }
}

@media (max-width: 600px) {
  .chart-wrapper { padding: 18px 12px 22px; border-radius: 22px; }

  .chart-title { font-size: clamp(1.55rem, 4.5vw + 0.9rem, 2.2rem); }
  .chart-subtitle { font-size: 13px; margin-bottom: 18px; }

  .member-card { padding: 12px 12px; gap: 10px; border-radius: 16px; }
  .avatar-wrapper { width: 54px; height: 54px; }
  .member-name { font-size: 14px; }
  .member-role { font-size: 0.82rem; }
}

@media (max-width: 420px) {
  .member-card { padding: 11px 10px; gap: 9px; }
  .avatar-wrapper { width: 50px; height: 50px; }
  .member-name { font-size: 13.5px; }
  .member-role { font-size: 0.78rem; }

  .bank-logo { width: 18px; height: 18px; }
  .bank-name { font-size: 0.78rem; }
}

/* ถ้าเครื่องไม่มี hover จริงๆ ให้ไม่ต้องยกการ์ด */
@media (hover: none) and (pointer: coarse) {
  .member-card:hover {
    transform: none;
    box-shadow: 0 14px 34px rgba(0, 0, 0, 0.55);
  }
}
</style>
