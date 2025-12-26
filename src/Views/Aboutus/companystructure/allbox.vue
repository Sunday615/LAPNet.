<!-- LaoStyleOrgChart_CEO.vue -->

<template>
  <main_navbar
    title="ໂຄງຮ່າງການຈັດຕັ້ງ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ກ່ຽວກັບພວກເຮົາ',
      'ໂຄງຮ່າງການຈັດຕັ້ງ',
      'ຄະນະບໍລິຫານ ພາຍໃນບໍລິສັດ',
    ]"
    background-image="/aboutus/navigatormission-bg.png"
  />
  <div class="navbarcompany">
    <cpn_navbar />
  </div>

  <div class="org-page">
    <div class="org-container" ref="root">
      <!-- TOP HEADER BAR -->
      <header class="org-header">
        <div class="org-header-left">
          <h1 class="org-title-lao">ຄະນະບໍລິຫານ ພາຍໃນບໍລິສັດ</h1>
        </div>
        <div class="org-header-right">
          <div class="org-logo-circle">
            <img src="/logolapnet/fullcircle.png" alt="LAPNet logo" />
          </div>
          <div class="org-header-en">
            LAO NATIONAL<br />
            PAYMENT NETWORK CO., LTD
          </div>
        </div>
      </header>

      <!-- ORG CHART FRAME -->
      <section class="org-frame">
        <div
          v-for="(row, rowIndex) in rowsToRender"
          :key="rowIndex"
          :class="['org-row', `org-row--${rowIndex}`]"
        >
          <article
            v-for="person in row"
            :key="person.id"
            class="org-card"
            :class="[
              { 'org-card--head': rowIndex >= 2 }, // Heads zone (rowIndex>=2) ทั้ง desktop+mobile
              { 'org-card--row4': !!person.isRow4 }, // ✅ ใช้ flag ของ person (สำคัญตอน mobile ที่ merge row)
            ]"
          >
            <!-- AVATAR -->
            <div class="org-avatar-wrapper">
              <div class="org-avatar-ring">
                <div class="org-avatar-inner">
                  <img
                    v-if="person.photo"
                    :src="person.photo"
                    :alt="person.name"
                    class="org-avatar-img"
                  />
                  <span v-else class="org-avatar-placeholder">
                    {{ getInitials(person.name) }}
                  </span>
                </div>
              </div>
            </div>

            <!-- TEXT -->
            <div class="org-card-body">
              <h2 class="org-card-name">{{ person.name }}</h2>
              <p class="org-card-role">{{ person.role }}</p>
            </div>
          </article>
        </div>
      </section>
    </div>
  </div>

  <secondfooter />
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
import { gsap } from "gsap";
import main_navbar from "../../../components/miannavbar/main_navbar.vue";
import cpn_navbar from "./navbarcompany/cpn_navbar.vue";
import secondfooter from "../../../components/footer/mainfooter/secondfooter.vue";

const root = ref(null);

/**
 * ✅ baseRows = โครงสร้างจริง (desktop)
 * ✅ rowsToRender = โครงสร้างที่ใช้ render (mobile 1 column จะ reorder)
 */
const baseRows = [
  // Row 1 – CEO
  [
    {
      id: 1,
      name: "ທ່ານ ສີສະໝອນ ສຣິດທິຣາດ",
      role: "ຜູ້ອຳນວຍການ ",
      photo: "/aboutus/company/lapnet_employee_image/CEO/CEO.png",
    },
  ],

  // Row 2 – COO
  [
    {
      id: 2,
      name: "ທ່ານ ນາງ ນີວະສອນ ມາລາທິບ",
      role: "ຮອງຜູ້ອຳນວຍການ ",
      photo: "/aboutus/company/lapnet_employee_image/CEO/CFO.png",
    },
  ],

  // Row 3 – Heads (5 คน)
  [
    {
      id: 3,
      name: "ທ່ານ ສັນຕິພາບ ປານມະໄລທອງ ",
      role: "ຫົວໜ້າພະແນກຫ້ອງການ",
      photo: "/aboutus/company/lapnet_employee_image/aministration/1.png",
    },
    {
      id: 4,
      name: "ທ່ານ ວັດທະນາ ວໍລະບຸດ",
      role: "ຫົວໜ້າພະແນກບັນຊີ ແລະ ການເງິນ",
      photo: "/aboutus/company/lapnet_employee_image/finance/1.png",
    },
    {
      id: 5,
      name: "ທ່ານ ພູເຂົາທອງ ເມືອງວົງ",
      role: "ຫົວໜ້າພະແນກໄອທີ",
      photo: "/aboutus/company/lapnet_employee_image/IT/1.png",
    },
    {
      id: 6,
      name: "ທ່ານ ເດດມີໄຊ ວັນນະຈິດ",
      role: "ຫົວໜ້າພະແນກດໍາເນີນງານ",
      photo: "/aboutus/company/lapnet_employee_image/operation/1.png",
    },
    {
      id: 7,
      name: "ກວດສອບພາຍໃນ",
      role: "Internal Audit",
      photo: "/aboutus/company/avarta.png",
    },
  ],

  // Row 4 – (1 node) ✅ ใส่ flag isRow4 เพื่อเอาไปจัด style/reorder ตอน mobile
  [
    {
      id: 8,
      name: "ທ່ານ ນາງ ອາລີພອນ ເພັງສະຫວັດດີ",
      role: "ຮອງຫົວໜ້າພະແນກຫ້ອງການ",
      photo: "/aboutus/company/lapnet_employee_image/aministration/2.png",
      isRow4: true,
    },
  ],
];

// ✅ ตรวจโหมด 1 column (<= 640px)
const isOneColumn = ref(false);
let mq = null;

const handleMQ = (e) => {
  isOneColumn.value = !!e.matches;
};

// ✅ rows ที่ใช้ render จริง
// - Desktop: [row1,row2,row3,row4]
// - Mobile 1 column: ย้าย id:8 ไปอยู่ใต้ id:3 (หลังตัวแรกของ row3) แล้วรวมเป็นแถวเดียว (ไม่มี row4 แยก)
const rowsToRender = computed(() => {
  if (!isOneColumn.value) return baseRows;

  const r1 = baseRows[0] ?? [];
  const r2 = baseRows[1] ?? [];
  const r3 = baseRows[2] ? [...baseRows[2]] : [];
  const r4First = baseRows[3]?.[0];

  if (r4First && r3.length > 0) {
    r3.splice(1, 0, r4First); // ✅ หลัง id:3 ทันที
  } else if (r4First) {
    r3.push(r4First);
  }

  return [r1, r2, r3];
});

// initials fallback (2 ตัวแรก)
const getInitials = (name) => (name || "").trim().slice(0, 2) || "?";

let gsapCtx;

onMounted(() => {
  // matchMedia (กัน SSR)
  if (typeof window !== "undefined") {
    mq = window.matchMedia("(max-width: 640px)");
    isOneColumn.value = mq.matches;

    if (mq.addEventListener) mq.addEventListener("change", handleMQ);
    else mq.addListener(handleMQ);
  }

  // GSAP
  gsapCtx = gsap.context(() => {
    const tl = gsap.timeline({ defaults: { ease: "power3.out" } });

    tl.from(".org-container", {
      opacity: 0,
      y: 48,
      scale: 0.97,
      duration: 0.8,
    })
      .from(".org-header-left", { x: -40, opacity: 0, duration: 0.6 }, "-=0.4")
      .from(".org-header-right", { x: 40, opacity: 0, duration: 0.6 }, "-=0.5")
      .from(".org-frame", { opacity: 0, y: 24, duration: 0.7 }, "-=0.25")
      .from(".org-row", { opacity: 0, y: 40, duration: 0.7, stagger: 0.16 }, "-=0.2")
      .from(
        ".org-card",
        {
          opacity: 0,
          y: 30,
          rotateX: -18,
          transformOrigin: "50% 100%",
          duration: 0.9,
          stagger: { each: 0.08, from: "center" },
        },
        "-=0.55"
      )
      .from(
        ".org-avatar-ring",
        {
          scale: 0.5,
          opacity: 0,
          duration: 0.55,
          stagger: { each: 0.08, from: "center" },
        },
        "-=0.55"
      );
  }, root.value);
});

onBeforeUnmount(() => {
  if (gsapCtx) gsapCtx.revert();

  if (mq) {
    if (mq.removeEventListener) mq.removeEventListener("change", handleMQ);
    else mq.removeListener(handleMQ);
  }
});
</script>

<style scoped>
/* NAVBAR WRAPPER */
.navbarcompany {
  width: 100%;
  height: 20vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  background-color: #f5f7fb;
}

/* PAGE BACKGROUND */
.org-page {
  min-height: 100vh;
  padding: 56px 24px;
  background: radial-gradient(circle at top, #dde7ff, #eef3fb 45%, #e2e8f0);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  box-sizing: border-box;
}

/* MAIN CONTAINER */
.org-container {
  width: 100%;
  max-width: 1680px;
  background: #ffffff;
  border-radius: 14px;
  box-shadow: 0 20px 60px rgba(15, 23, 42, 0.18);
  overflow: hidden;
}

/* HEADER BAR */
.org-header {
  display: flex;
  justify-content: space-between;
  align-items: stretch;
  background: linear-gradient(135deg, #0f2d55, #123765);
  color: #ffffff;
  padding: 26px 56px;
}

.org-header-left {
  display: flex;
  align-items: center;
}

.org-title-lao {
  margin: 0;
  font-size: 1.9rem;
  font-weight: 600;
}

.org-header-right {
  display: flex;
  align-items: center;
  gap: 20px;
}

.org-logo-circle {
  width: 82px;
  height: 82px;
  border-radius: 999px;
  border: 3px solid #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: radial-gradient(circle at 30% 0, #4f8efc, #0a2b5c);
}

.org-logo-circle img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.org-header-en {
  font-size: 0.95rem;
  font-weight: 600;
  letter-spacing: 0.18em;
  text-transform: uppercase;
}

/* FRAME AROUND NODES */
.org-frame {
  margin: 40px 56px 46px;
  padding: 70px 40px 54px;
  border-radius: 16px;
  border: 2px dashed rgba(148, 163, 184, 0.8);
  background: radial-gradient(circle at top, #f8fafc, #edf2ff 40%, #f9fafb);
  perspective: 1680px;

  /* ✅ Row4 Variables (ใช้กับการ์ดที่มี isRow4=true แม้ตอน mobile จะถูก merge เข้า row3) */
  --row4-card-width: 265px;
  --row4-card-padding: 56px 28px 22px;
  --row4-avatar-size: 112px;
  --row4-avatar-padding: 9px;
  --row4-name-size: 1rem;
  --row4-role-size: 1rem;
}

/* ROWS */
.org-row {
  
  margin-top: 30px;
  display: flex;
  justify-content: center;
  gap: 22px;
  margin-bottom: 70px;
}

/* ✅ Row 4: desktop ให้ชิดซ้าย (จะมีเฉพาะตอน desktop เพราะ mobile จะ merge แล้วไม่มี row--3) */
.org-row--3 {
  justify-content: flex-start;
  margin-bottom: 0;
  
}

/* CARD (CEO/COO ใช้ style เดิม) */
.org-card {
  position: relative;
  width: 420px;
  max-width: 90%;
  background: radial-gradient(circle at top, #123765, #061429);
  border-radius: 20px;
  padding: 60px 40px 28px;
  color: #ffffff;
  text-align: center;
  box-shadow: 0 18px 40px rgba(15, 23, 42, 0.45);
  transform-style: preserve-3d;
  border: 1px solid rgba(148, 163, 184, 0.45);
  overflow: visible;
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1),
    box-shadow 0.35s cubic-bezier(0.16, 1, 0.3, 1),
    background 0.35s ease-out;
}

.org-card::before {
  content: "";
  position: absolute;
  inset: -1px;
  border-radius: inherit;
  opacity: 0;
  z-index: -1;
  transition: opacity 0.35s ease-out;
}

/* hover */
.org-card:hover {
  transform: translateY(-12px) rotateX(6deg) rotateY(-3deg) scale(1.02);
  box-shadow: 0 28px 70px rgba(15, 23, 42, 0.65);
  background: radial-gradient(circle at top, #1a4174, #050c1a);
}

.org-card:hover::before {
  opacity: 1;
}

/* ✅ Heads style (row3/row4 zone) */
.org-card--head {
  width: 320px;
  margin-top: 30px;
  padding: 56px 28px 22px;
  background: #123765 !important;
  border: 1px solid rgba(255, 255, 255, 0.14);
  box-shadow: 0 16px 36px rgba(15, 23, 42, 0.42);
}

.org-card--head:hover {
  background: #123765 !important;
  transform: translateY(-10px) rotateX(5deg) rotateY(-2deg) scale(1.015);
  box-shadow: 0 26px 60px rgba(15, 23, 42, 0.6);
}

/* ✅ Row4 override: ปรับขนาดได้ตามต้องการ (ทำงานผ่าน isRow4 flag) */
.org-card--row4 {
  width: var(--row4-card-width, 300px);
  padding: var(--row4-card-padding, 56px 28px 22px);

  margin-top: 40px;
}

.org-card--row4 .org-avatar-ring {
  width: var(--row4-avatar-size, 112px);
  height: var(--row4-avatar-size, 112px);
  padding: var(--row4-avatar-padding, 9px);
}

.org-card--row4 .org-card-name {
  font-size: var(--row4-name-size, 1rem);
}
.org-card--row4 .org-card-role {
  font-size: var(--row4-role-size, 1rem);
}

/* AVATAR */
.org-avatar-wrapper {
  position: absolute;
  top: -56px;
  left: 50%;
  transform: translateX(-50%);
}

.org-avatar-ring {
  width: 132px;
  height: 132px;
  border-radius: 999px;
  padding: 10px;
  background: #123765;
  box-shadow: 0 16px 30px rgba(15, 23, 42, 0.55);
}

/* ลดวงแหวนใน heads (รวมถึง Row4 card ถ้าไม่ได้ override) */
.org-card--head .org-avatar-ring {
  width: 112px;
  height: 112px;
  padding: 9px;
}

.org-avatar-inner {
  width: 100%;
  height: 100%;
  border-radius: inherit;
  background: #020617;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.org-avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.org-avatar-placeholder {
  font-size: 1.4rem;
  font-weight: 600;
  color: #e5edff;
}

/* TEXT (default) */
.org-card-name {
  margin: 18px 0 6px;
  font-size: var(--fs-md);
  font-weight: 600;
}

.org-card-role {
  margin: 0;
  font-size: 0.92rem;
  opacity: 0.94;
}

/* heads text */
.org-card--head .org-card-name {
  font-size: 1.02rem;
}
.org-card--head .org-card-role {
  font-size: 0.88rem;
}

/* RESPONSIVE */
@media (max-width: 1200px) {
  .org-row--2 {
    flex-wrap: wrap;
    gap: 28px;
  }

  /* ✅ responsive: Row4 (ถ้ามีแถวแยกบน desktop) ให้มาอยู่กลาง */
  .org-row--3 {
    justify-content: center;
  }
}

@media (max-width: 900px) {
  .org-frame {
    margin: 28px 24px 32px;
    padding: 56px 20px 42px;
    --row4-card-width: 320px;
  }

  .org-row {
    flex-wrap: wrap;
    margin-bottom: 50px;
  }

  .org-row--3 {
    justify-content: center;
    margin-bottom: 0;
  }

  .org-card {
    margin-top: 80px;
    width: 360px;
  }

  .org-card--head {
    width: 320px;
  }

  .org-card--row4 {
  
    width: var(--row4-card-width, 320px);
    padding: var(--row4-card-padding, 56px 28px 22px);
  }
}

@media (max-width: 640px) {
  .org-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
    padding: 22px 24px;
  }

  .org-header-en {
    font-size: 13px;
  }

  .org-logo-circle {
    width: 60px;
    height: 60px;
  }

  .org-card {
    width: 100%;
    max-width: 340px;
    padding-inline: 24px;
    margin-top: 80px;
  }

  .org-card--head {
    max-width: 340px;
  }

  .org-frame {
    margin: 22px 16px 30px;
    padding: 50px 16px 34px;
    --row4-card-width: 100%;
  }
}
</style>
