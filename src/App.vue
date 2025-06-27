<script setup lang="ts">
import { RouterView } from 'vue-router'
import MainNavigation from './components/MainNavigation.vue'
</script>

<template>
  <div class="app">
    <MainNavigation />
    <div class="container">
      <main class="main-content">
        <RouterView />
      </main>
    </div>
  </div>
</template>

<style>
@import '@/assets/base.css';

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  width: 100%;
  display: flex;
  justify-content: center;
  flex: 1;
}

.main-content {
  flex: 1;
  padding-top: 80px; /* 导航栏高度 */
  width: 100%;
  max-width: 1600px;
  padding-left: 1rem;
  padding-right: 1rem;
}

@media (min-width: 768px) {
  .main-content {
    padding-left: 2rem;
    padding-right: 2rem;
  }
}

@media (min-width: 1200px) {
  .main-content {
    padding-left: 4rem;
    padding-right: 4rem;
  }
}
</style>
