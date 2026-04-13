<template>
  <div class="app">
    <BackgroundEffect />
    <main class="container">
      <!-- Импортируем переменные из конфига -->
      <ProfileHeader
        :avatar="profile.avatar"
        :name="profile.name"
        :handle="profile.handle"
        :bio="profile.bio"
      />
      <!-- и ссылки тоже -->
      <LinkList :links="links" />
      
      <footer class="footer">
        <span>made with <span class="heart">♥</span> & vaporwave</span>
      </footer>
    </main>
  </div>
</template>

<script setup>
import BackgroundEffect from './components/BackgroundEffect.vue'
import ProfileHeader from './components/ProfileHeader.vue'
import LinkList from './components/LinkList.vue'
import { profile, links } from './data/links.js'
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap');

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:root {
  --neon-pink:    #ff2d78;
  --neon-cyan:    #00f5ff;
  --neon-purple:  #bf5fff;
  --neon-yellow:  #ffe600;
  --bg-dark:      #0a0010;
  --bg-mid:       #120025;
  --card-bg:      rgba(255, 255, 255, 0.04);
  --card-border:  rgba(255, 255, 255, 0.12);
  --text-primary: #f0e6ff;
  --text-muted:   #9b7fc7;
  --font-display: 'Orbitron', monospace;
  --font-mono:    'Share Tech Mono', monospace;
}

html, body {
  height: 100%;
  background: var(--bg-dark);
  color: var(--text-primary);
  font-family: var(--font-mono);
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
}

#app {
  min-height: 100vh;
}

.app {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: flex-start;
  justify-content: center;
}

.container {
  position: relative;
  z-index: 10;
  width: 100%;
  max-width: 520px;
  padding: 60px 20px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}

.footer {
  margin-top: 40px;
  font-size: 12px;
  color: var(--text-muted);
  letter-spacing: 0.08em;
  opacity: 0.6;
}

.heart {
  color: var(--neon-pink);
  animation: heartbeat 1.8s ease-in-out infinite;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.3); }
}
</style>
