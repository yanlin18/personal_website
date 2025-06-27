<script setup lang="ts">
import { ref } from 'vue'
import ThemeToggle from './ThemeToggle.vue'
import { RouterLink } from 'vue-router'

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}
</script>

<template>
  <nav class="main-nav">
    <div class="nav-container">
      <div class="logo-container">
        <RouterLink to="/" class="logo">
          <span class="logo-text">yanlin</span>
          <span class="logo-dot"></span>
        </RouterLink>
      </div>

      <div class="nav-controls">
        <ThemeToggle />
        <button class="menu-toggle" @click="toggleMenu" :class="{ 'is-active': isMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>

      <div class="nav-links" :class="{ 'is-open': isMenuOpen }">
        <RouterLink to="/" class="nav-link" @click="isMenuOpen = false">首页</RouterLink>
        <RouterLink to="/navigation" class="nav-link" @click="isMenuOpen = false">导航</RouterLink>
        <RouterLink to="/profile" class="nav-link" @click="isMenuOpen = false">个人名片</RouterLink>
        <RouterLink to="/music" class="nav-link" @click="isMenuOpen = false">音乐</RouterLink>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.main-nav {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 100;
  backdrop-filter: blur(10px);
  background-color: rgba(var(--color-background-rgb), 0.8);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: center;
}

.nav-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  max-width: 1600px;
  width: 100%;
  position: relative;
}

.logo-container {
  z-index: 10;
}

.logo {
  display: flex;
  align-items: center;
  text-decoration: none;
  font-weight: bold;
  font-size: 1.5rem;
  color: var(--color-primary);
  letter-spacing: 1px;
}

.logo-text {
  margin-right: 5px;
}

.logo-dot {
  display: inline-block;
  width: 8px;
  height: 8px;
  background-color: var(--color-accent);
  border-radius: 50%;
  animation: pulse 2s infinite;
}

.nav-controls {
  display: flex;
  align-items: center;
  gap: 1rem;
  z-index: 10;
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  width: 30px;
  height: 22px;
  position: relative;
  z-index: 10;
}

.menu-toggle span {
  display: block;
  width: 100%;
  height: 3px;
  background-color: var(--color-primary);
  position: absolute;
  left: 0;
  transition: all 0.3s ease;
}

.menu-toggle span:nth-child(1) {
  top: 0;
}

.menu-toggle span:nth-child(2) {
  top: 9px;
}

.menu-toggle span:nth-child(3) {
  top: 18px;
}

.menu-toggle.is-active span:nth-child(1) {
  transform: rotate(45deg);
  top: 9px;
}

.menu-toggle.is-active span:nth-child(2) {
  opacity: 0;
}

.menu-toggle.is-active span:nth-child(3) {
  transform: rotate(-45deg);
  top: 9px;
}

.nav-links {
  display: flex;
  gap: 2rem;
}

.nav-link {
  color: var(--color-text);
  text-decoration: none;
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background-color: var(--color-primary);
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: var(--color-primary);
}

.nav-link:hover::after {
  width: 100%;
}

.router-link-active {
  color: var(--color-primary) !important;
}

.router-link-active::after {
  width: 100%;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  .nav-links {
    position: fixed;
    top: 0;
    right: -100%;
    width: 100%;
    height: 100vh;
    background-color: var(--color-background);
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transition: right 0.5s ease;
    z-index: 5;
  }

  .nav-links.is-open {
    right: 0;
  }

  .nav-link {
    font-size: 1.5rem;
  }
}
</style>
