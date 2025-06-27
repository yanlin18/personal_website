<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'

const isDarkTheme = ref(false)

// 切换主题
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
  if (isDarkTheme.value) {
    document.documentElement.classList.add('dark-theme')
    localStorage.setItem('theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark-theme')
    localStorage.setItem('theme', 'light')
  }
}

// 初始化主题
onMounted(() => {
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark') {
    isDarkTheme.value = true
    document.documentElement.classList.add('dark-theme')
  }
})
</script>

<template>
  <div class="theme-toggle" @click="toggleTheme">
    <div class="toggle-track" :class="{ 'is-dark': isDarkTheme }">
      <div class="toggle-indicator">
        <span class="toggle-icon">
          <svg
            v-if="isDarkTheme"
            class="moon-icon"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          <svg
            v-else
            class="sun-icon"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle
              cx="12"
              cy="12"
              r="5"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.theme-toggle {
  cursor: pointer;
  position: relative;
  display: inline-block;
}

.toggle-track {
  background-color: var(--color-background-soft);
  border: 2px solid var(--color-border);
  border-radius: 30px;
  height: 32px;
  width: 60px;
  position: relative;
  transition: all 0.3s ease;
}

.toggle-track.is-dark {
  background-color: var(--color-secondary);
  border-color: var(--color-primary);
}

.toggle-indicator {
  position: absolute;
  height: 24px;
  width: 24px;
  background-color: var(--color-primary);
  border-radius: 50%;
  top: 2px;
  left: 2px;
  transform: translateX(0);
  transition: transform 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.toggle-track.is-dark .toggle-indicator {
  transform: translateX(28px);
  background-color: var(--color-accent);
}

.toggle-icon {
  color: var(--color-background);
  height: 14px;
  width: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}

svg {
  width: 14px;
  height: 14px;
}
</style>
