<template>
  <div class="bg" aria-hidden="true">
    <div class="bg__glow bg__glow--top" />
    <div class="bg__glow bg__glow--left" />
    <div class="bg__glow bg__glow--right" />
    <div class="bg__scanlines" />  <!-- сетка -->
    <div class="bg__grid-wrapper">
      <div class="bg__grid" />
    </div>
    <canvas ref="canvas" class="bg__particles" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref(null)
let animId = null

onMounted(() => {
  const c = canvas.value
  const ctx = c.getContext('2d')
  let W = 0, H = 0

  const particles = Array.from({ length: 55 }, () => ({
    x: Math.random(),
    y: Math.random(),
    r: Math.random() * 1.6 + 0.3,
    vx: (Math.random() - 0.5) * 0.00015,
    vy: (Math.random() - 0.5) * 0.00015,
    hue: Math.random() < 0.5 ? 300 : 185,
    alpha: Math.random() * 0.5 + 0.15,
  }))

  function resize() {
    W = c.width  = window.innerWidth
    H = c.height = window.innerHeight
  }
  resize()
  window.addEventListener('resize', resize)

  function draw() {
    ctx.clearRect(0, 0, W, H)
    for (const p of particles) {
      p.x += p.vx; p.y += p.vy
      if (p.x < 0) p.x = 1; if (p.x > 1) p.x = 0
      if (p.y < 0) p.y = 1; if (p.y > 1) p.y = 0

      ctx.beginPath()
      ctx.arc(p.x * W, p.y * H, p.r, 0, Math.PI * 2)
      ctx.fillStyle = `hsla(${p.hue}, 100%, 75%, ${p.alpha})`
      ctx.fill()
    }
    animId = requestAnimationFrame(draw)
  }
  draw()

  onUnmounted(() => {
    cancelAnimationFrame(animId)
    window.removeEventListener('resize', resize)
  })
})
</script>

<style scoped>
.bg {
  position: fixed;
  inset: 0;
  overflow: hidden;
  z-index: 0;
}

.bg__glow {
  position: absolute;
  border-radius: 50%;
  filter: blur(90px);
  pointer-events: none;
}
.bg__glow--top {
  top: -180px; left: 50%;
  transform: translateX(-50%);
  width: 700px; height: 420px;
  background: radial-gradient(ellipse, rgba(191, 95, 255, 0.28) 0%, transparent 70%);
}
.bg__glow--left {
  bottom: 20%; left: -160px;
  width: 380px; height: 380px;
  background: radial-gradient(ellipse, rgba(255, 45, 120, 0.2) 0%, transparent 70%);
  animation: driftL 14s ease-in-out infinite alternate;
}
.bg__glow--right {
  top: 30%; right: -120px;
  width: 320px; height: 320px;
  background: radial-gradient(ellipse, rgba(0, 245, 255, 0.18) 0%, transparent 70%);
  animation: driftR 11s ease-in-out infinite alternate;
}
@keyframes driftL { to { transform: translate(30px, -40px); } }
@keyframes driftR { to { transform: translate(-20px, 50px); } }

/* сетка */
.bg__scanlines {
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    to bottom,
    transparent 0px,
    transparent 2px,
    rgba(0, 0, 0, 0.12) 2px,
    rgba(0, 0, 0, 0.12) 4px
  );
  pointer-events: none;
}

.bg__grid-wrapper {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 55vh;
  perspective: 350px;
  overflow: hidden;
}
.bg__grid {
  width: 100%; height: 100%;
  transform: rotateX(60deg);
  transform-origin: bottom center;
  background-image:
    linear-gradient(rgba(191, 95, 255, 0.22) 1px, transparent 1px),
    linear-gradient(90deg, rgba(191, 95, 255, 0.22) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: scrollGrid 4s linear infinite;
  mask-image: linear-gradient(to bottom, transparent 0%, rgba(0,0,0,0.8) 40%, black 100%);
  -webkit-mask-image: linear-gradient(to bottom, transparent 0%, rgba(0,0,0,0.8) 40%, black 100%);
}
@keyframes scrollGrid {
  from { background-position: 0 0; }
  to   { background-position: 0 60px; }
}

.bg__particles {
  position: absolute;
  inset: 0;
  width: 100%; height: 100%;
  pointer-events: none;
}
</style>
