<script setup lang="ts">
import { onMounted, ref } from 'vue'

const heroTitle = ref('开发者导航 & 个人空间')
const heroText = ref('欢迎来到雁翎的小窝！')

// 视差效果
const parallaxElements = ref<HTMLElement[]>([])
const handleMouseMove = (e: MouseEvent) => {
  const x = e.clientX / window.innerWidth
  const y = e.clientY / window.innerHeight

  parallaxElements.value.forEach((el, i) => {
    const speed = (i + 1) * 15
    const xOffset = (x - 0.5) * speed
    const yOffset = (y - 0.5) * speed
    el.style.transform = `translate(${xOffset}px, ${yOffset}px)`
  })
}

onMounted(() => {
  parallaxElements.value = Array.from(document.querySelectorAll('.parallax-element'))
  document.addEventListener('mousemove', handleMouseMove)
})
</script>

<template>
  <div class="home">
    <section class="hero-section">
      <div class="hero-content">
        <h1 class="hero-title">{{ heroTitle }}</h1>
        <p class="hero-text">{{ heroText }}</p>
        <div class="hero-buttons">
          <RouterLink to="/navigation" class="hero-button primary">
            <span>开始探索</span>
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path
                d="M5 12h14M12 5l7 7-7 7"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </RouterLink>
          <RouterLink to="/profile" class="hero-button secondary">
            <span>关于我</span>
          </RouterLink>
        </div>
      </div>

      <div class="hero-graphics">
        <div class="parallax-container">
          <div class="parallax-element circle circle-1"></div>
          <div class="parallax-element circle circle-2"></div>
          <div class="parallax-element triangle"></div>
          <div class="parallax-element square"></div>
          <div class="parallax-element dots"></div>
          <div class="parallax-element circle circle-3"></div>
          <div class="parallax-element dots dots-2"></div>
          <div class="parallax-element square square-2"></div>
        </div>
      </div>
    </section>

    <section class="features-section">
      <h2 class="section-title">功能特色</h2>
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path
                d="M12 5v14M5 12h14"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </div>
          <h3 class="feature-title">前端导航</h3>
          <p class="feature-description">精选前端优质网站，快速找到需要的资源</p>
        </div>

        <div class="feature-card">
          <div class="feature-icon">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path
                d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </div>
          <h3 class="feature-title">个人名片</h3>
          <p class="feature-description">展示个人信息与技能</p>
        </div>

        <div class="feature-card">
          <div class="feature-icon">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path
                d="M9 18V5l12 13V5M3 8v8"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </div>
          <h3 class="feature-title">音乐播放</h3>
          <p class="feature-description">享受编程时的音乐氛围，提高工作效率</p>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.home {
  padding-bottom: 4rem;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero-section {
  display: flex;
  min-height: calc(100vh - 80px);
  position: relative;
  overflow: hidden;
  flex-direction: column;
  width: 100%;
}

.hero-content {
  width: 100%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem 1rem;
  z-index: 2;
  margin: 0 auto;
}

.hero-title {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  background: linear-gradient(45deg, var(--color-primary), var(--color-accent));
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: fadeInUp 1s ease-out;
}

.hero-text {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  max-width: 100%;
  color: var(--color-text);
  animation: fadeInUp 1s ease-out 0.2s both;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  animation: fadeInUp 1s ease-out 0.4s both;
}

.hero-button {
  padding: 0.8rem 1.5rem;
  border-radius: 50px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.hero-button.primary {
  background: linear-gradient(45deg, var(--color-primary), var(--color-primary-soft));
  color: white;
  box-shadow: 0 4px 15px rgba(66, 184, 131, 0.4);
}

.hero-button.primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(66, 184, 131, 0.6);
}

.hero-button.secondary {
  background: transparent;
  color: var(--color-text);
  border: 2px solid var(--color-border);
}

.hero-button.secondary:hover {
  background-color: var(--color-background-soft);
  border-color: var(--color-border-hover);
}

.hero-button svg {
  width: 20px;
  height: 20px;
}

.hero-graphics {
  flex: 1;
  position: relative;
  width: 100%;
}

.parallax-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.parallax-element {
  position: absolute;
  transition: transform 0.1s ease-out;
}

.circle {
  border-radius: 50%;
}

.circle-1 {
  width: 150px;
  height: 150px;
  background: linear-gradient(135deg, var(--color-primary-soft), var(--color-primary));
  top: 20%;
  right: 45%;
  opacity: 0.7;
}

.circle-2 {
  width: 80px;
  height: 80px;
  background: linear-gradient(135deg, var(--color-accent), var(--color-accent-soft));
  top: 60%;
  right: 60%;
  opacity: 0.6;
}

.circle-3 {
  width: 120px;
  height: 120px;
  background: linear-gradient(135deg, var(--color-secondary), var(--color-primary-soft));
  top: 30%;
  right: 15%;
  opacity: 0.5;
}

.triangle {
  width: 0;
  height: 0;
  border-left: 70px solid transparent;
  border-right: 70px solid transparent;
  border-bottom: 120px solid var(--color-secondary);
  opacity: 0.5;
  top: 15%;
  right: 30%;
}

.square {
  width: 100px;
  height: 100px;
  background: var(--color-accent-soft);
  transform: rotate(45deg);
  top: 45%;
  right: 25%;
  opacity: 0.3;
}

.square-2 {
  width: 60px;
  height: 60px;
  background: var(--color-primary);
  transform: rotate(30deg);
  top: 65%;
  right: 20%;
  opacity: 0.4;
}

.dots {
  width: 160px;
  height: 160px;
  background-image: radial-gradient(var(--color-primary) 2px, transparent 2px);
  background-size: 20px 20px;
  top: 55%;
  right: 40%;
  opacity: 0.5;
}

.dots-2 {
  width: 200px;
  height: 200px;
  background-image: radial-gradient(var(--color-accent) 2px, transparent 2px);
  background-size: 25px 25px;
  top: 15%;
  right: 5%;
  opacity: 0.3;
}

.features-section {
  padding: 4rem 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.section-title {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 3rem;
  color: var(--color-heading);
  position: relative;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: -15px;
  left: 50%;
  width: 60px;
  height: 3px;
  background: linear-gradient(45deg, var(--color-primary), var(--color-accent));
  transform: translateX(-50%);
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  margin: 0 auto;
  width: 100%;
}

.feature-card {
  background-color: var(--color-background-soft);
  border-radius: 20px;
  padding: 2rem;
  transition: all 0.3s ease;
  border: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.feature-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  border-color: var(--color-primary);
}

.feature-icon {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: var(--color-background-mute);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 1.5rem;
  color: var(--color-primary);
}

.feature-icon svg {
  width: 30px;
  height: 30px;
}

.feature-title {
  font-size: 1.3rem;
  margin-bottom: 1rem;
  color: var(--color-heading);
}

.feature-description {
  color: var(--color-text);
  line-height: 1.6;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 1200px) {
  .features-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .hero-section {
    flex-direction: column;
  }

  .hero-content {
    padding: 2rem 0;
  }

  .hero-title {
    font-size: 2.2rem;
  }

  .hero-graphics {
    height: 300px;
  }
}

@media (min-width: 992px) {
  .hero-section {
    flex-direction: row;
  }

  .hero-content {
    flex: 1;
    margin: 0;
    max-width: 50%;
    padding: 2rem;
  }
}
</style>
