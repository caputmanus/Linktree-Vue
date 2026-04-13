<template>
  <header class="profile">
    <div class="profile__avatar-ring">
      <div class="profile__avatar-ring-inner">
        <img
          v-if="avatar"
          :src="avatar"
          :alt="name"
          class="profile__avatar-img"
        />
        <div v-else class="profile__avatar-placeholder">
          {{ initials }}
        </div>
      </div>
    </div>
    <h1 class="profile__name">{{ name }}</h1>
    <p class="profile__handle">@{{ handle }}</p>
    <p class="profile__bio">{{ bio }}</p>
    <div class="profile__divider">
      <span class="profile__divider-line" />
      <span class="profile__divider-diamond">◆</span>
      <span class="profile__divider-line" />
    </div>
  </header>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  avatar: { type: String, default: '' },
  name:   { type: String, required: true },
  handle: { type: String, required: true },
  bio:    { type: String, default: '' },
})

const initials = computed(() => {
  let result = '';
  let isWordStart = true;

  for (let i = 0; i < props.name.length; i++) {
    const char = props.name[i];

    if (char === ' ') {
      isWordStart = true;
    } else if (isWordStart) {
      result += char.toUpperCase();
      isWordStart = false;

      if (result.length === 2) break;
    }
  }

  return result;
})
</script>

<style scoped>
.profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 10px;
  animation: fadeDown 0.7s ease both;
  margin-bottom: 36px;
}

.profile__avatar-ring {
  width: 100px; height: 100px;
  border-radius: 50%;
  padding: 3px;
  background: conic-gradient(
    var(--neon-pink) 0%,
    var(--neon-purple) 33%,
    var(--neon-cyan) 66%,
    var(--neon-pink) 100%
  ); /* конфиг в App.vue */
  animation: spinRing 6s linear infinite;
  margin-bottom: 6px;
  filter: drop-shadow(0 0 14px rgba(191, 95, 255, 0.6));
}
.profile__avatar-ring-inner {
  width: 100%; height: 100%;
  border-radius: 50%;
  background: var(--bg-dark);
  padding: 4px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
.profile__avatar-img {
  width: 100%; height: 100%;
  border-radius: 50%;
  object-fit: cover;
}
.profile__avatar-placeholder {
  font-family: var(--font-display);
  font-size: 28px;
  font-weight: 900;
  background: linear-gradient(135deg, var(--neon-pink), var(--neon-cyan));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.profile__name {
  font-family: var(--font-display);
  font-size: clamp(20px, 5vw, 26px);
  font-weight: 900;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  background: linear-gradient(90deg, var(--neon-pink) 0%, var(--neon-cyan) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  filter: drop-shadow(0 0 8px rgba(255, 45, 120, 0.45));
  line-height: 1.2;
}

.profile__handle {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--neon-purple);
  letter-spacing: 0.12em;
  opacity: 0.9;
}

.profile__bio {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.7;
  max-width: 320px;
  letter-spacing: 0.03em;
  margin-top: 4px;
}

.profile__divider {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 8px;
  width: 100%;
  max-width: 300px;
}
.profile__divider-line {
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(191, 95, 255, 0.5), transparent);
}
.profile__divider-diamond {
  font-size: 8px;
  color: var(--neon-purple);
  opacity: 0.7;
}

@keyframes spinRing {
  to { transform: rotate(360deg); }
}
@keyframes fadeDown {
  from { opacity: 0; transform: translateY(-24px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>
