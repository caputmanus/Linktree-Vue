<template>
  <a
    :href="link.url"
    target="_blank"
    rel="noopener noreferrer"
    class="card"
    :class="`card--${link.color ?? 'purple'}`"
    :style="{ animationDelay: `${delay}ms` }"
    @mouseenter="hovered = true"
    @mouseleave="hovered = false"
  >
    <span class="card__glow" />
    <span class="card__icon" v-html="link.icon" />
    <span class="card__label">{{ link.label }}</span>
    <span class="card__arrow" :class="{ 'card__arrow--active': hovered }">▶</span>
    <span class="card__glitch" aria-hidden="true">{{ link.label }}</span>
  </a>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  link:  { type: Object, required: true },
  delay: { type: Number, default: 0 },
})

const hovered = ref(false)
</script>

<style scoped>
.card {
  position: relative;
  display: flex;
  align-items: center;
  gap: 16px;
  width: 100%;
  padding: 16px 22px;
  border-radius: 10px;
  text-decoration: none;
  font-family: var(--font-display);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-primary);
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  cursor: pointer;
  overflow: hidden;
  transition:
    transform 0.18s cubic-bezier(.34, 1.56, .64, 1),
    border-color 0.2s ease,
    box-shadow 0.2s ease;
  animation: slideIn 0.5s cubic-bezier(.34, 1.2, .64, 1) both;
  -webkit-tap-highlight-color: transparent;
}

.card--pink   { --c: var(--neon-pink);   }
.card--cyan   { --c: var(--neon-cyan);   }
.card--purple { --c: var(--neon-purple); }
.card--yellow { --c: var(--neon-yellow); }
/* конфиг в App.vue */

.card:hover {
  transform: translateY(-3px) scale(1.015);
  border-color: var(--c, var(--neon-purple));
  box-shadow:
    0 0 18px color-mix(in srgb, var(--c, var(--neon-purple)) 45%, transparent),
    0 0 40px color-mix(in srgb, var(--c, var(--neon-purple)) 18%, transparent),
    inset 0 0 20px color-mix(in srgb, var(--c, var(--neon-purple)) 8%, transparent);
}
.card:active {
  transform: translateY(0) scale(0.98);
}

/* ── Glow sweep on hover ── */
.card__glow {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    105deg,
    transparent 40%,
    color-mix(in srgb, var(--c, var(--neon-purple)) 12%, transparent) 50%,
    transparent 60%
  );
  transform: translateX(-100%);
  transition: transform 0.5s ease;
  pointer-events: none;
}
.card:hover .card__glow {
  transform: translateX(100%);
}

/* ── Icon ── */
.card__icon {
  font-size: 20px;
  flex-shrink: 0;
  line-height: 1;
  filter: drop-shadow(0 0 6px var(--c, var(--neon-purple)));
  transition: filter 0.2s ease;
}
.card:hover .card__icon {
  filter: drop-shadow(0 0 12px var(--c, var(--neon-purple)));
}

/* ── Label ── */
.card__label {
  flex: 1;
  position: relative;
  z-index: 1;
  transition: color 0.2s ease;
}
.card:hover .card__label {
  color: var(--c, var(--text-primary));
}

/* ── Arrow ── */
.card__arrow {
  font-size: 10px;
  color: var(--c, var(--neon-purple));
  opacity: 0;
  transform: translateX(-6px);
  transition: opacity 0.2s ease, transform 0.2s ease;
}
.card__arrow--active {
  opacity: 1;
  transform: translateX(0);
}

/* ── Glitch ── */
.card__glitch {
  position: absolute;
  left: 22px;
  top: 50%;
  transform: translateY(-50%);
  font-family: var(--font-display);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--c, var(--neon-purple));
  opacity: 0;
  pointer-events: none;
  /* offset behind the real label, triggered only on hover */
}
.card:hover .card__glitch {
  animation: glitch 0.3s steps(2) forwards;
}

@keyframes glitch {
  0%   { opacity: 0.6; clip-path: inset(0 0 80% 0); transform: translateY(-50%) translateX(-2px); }
  33%  { opacity: 0.4; clip-path: inset(40% 0 30% 0); transform: translateY(-50%) translateX(3px); }
  66%  { opacity: 0.3; clip-path: inset(70% 0 5% 0);  transform: translateY(-50%) translateX(-1px); }
  100% { opacity: 0;   clip-path: inset(0 0 100% 0); }
}

@keyframes slideIn {
  from { opacity: 0; transform: translateX(-30px); }
  to   { opacity: 1; transform: translateX(0); }
}
</style>
