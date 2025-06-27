<script setup lang="ts">
import { ref, computed } from 'vue'

interface WebsiteCategory {
  id: string
  name: string
  icon: string
}

interface Website {
  id: string
  title: string
  url: string
  description: string
  categoryId: string
  logo?: string
}

// 网站分类
const categories = ref<WebsiteCategory[]>([
  { id: 'framework', name: '前端框架', icon: 'code' },
  { id: 'ui', name: 'UI组件库', icon: 'palette' },
  { id: 'tools', name: '开发工具', icon: 'tool' },
  { id: 'docs', name: '文档教程', icon: 'book' },
  { id: 'community', name: '社区论坛', icon: 'users' },
  { id: 'design', name: '设计资源', icon: 'image' },
])

// 网站数据
const websites = ref<Website[]>([
  // 前端框架
  {
    id: 'vue',
    title: 'Vue.js',
    url: 'https://vuejs.org/',
    description: '渐进式JavaScript框架，易学易用，性能出色，适用于构建用户界面',
    categoryId: 'framework',
    logo: '🟢',
  },
  {
    id: 'react',
    title: 'React',
    url: 'https://reactjs.org/',
    description: '用于构建用户界面的JavaScript库，由Facebook开发和维护',
    categoryId: 'framework',
    logo: '⚛️',
  },
  {
    id: 'angular',
    title: 'Angular',
    url: 'https://angular.io/',
    description: '由Google维护的TypeScript编写的开源Web应用框架',
    categoryId: 'framework',
    logo: '🔴',
  },
  {
    id: 'svelte',
    title: 'Svelte',
    url: 'https://svelte.dev/',
    description: '无虚拟DOM的编译型前端框架，直接操作DOM，提供极佳性能',
    categoryId: 'framework',
    logo: '🔶',
  },

  // UI组件库
  {
    id: 'antd',
    title: 'Ant Design',
    url: 'https://ant.design/',
    description: '企业级UI设计语言和React组件库，提供一致的用户体验',
    categoryId: 'ui',
    logo: '🐜',
  },
  {
    id: 'element',
    title: 'Element Plus',
    url: 'https://element-plus.org/',
    description: '基于Vue 3的组件库，提供了丰富的组件和主题定制能力',
    categoryId: 'ui',
    logo: '🧩',
  },
  {
    id: 'bootstrap',
    title: 'Bootstrap',
    url: 'https://getbootstrap.com/',
    description: '最流行的HTML、CSS和JS框架，用于开发响应式布局',
    categoryId: 'ui',
    logo: '🅱️',
  },
  {
    id: 'tailwind',
    title: 'Tailwind CSS',
    url: 'https://tailwindcss.com/',
    description: '功能类优先的CSS框架，用于快速构建自定义设计',
    categoryId: 'ui',
    logo: '🌬️',
  },

  // 开发工具
  {
    id: 'vscode',
    title: 'VS Code',
    url: 'https://code.visualstudio.com/',
    description: '微软开发的轻量级但功能强大的源代码编辑器',
    categoryId: 'tools',
    logo: '📝',
  },
  {
    id: 'webpack',
    title: 'Webpack',
    url: 'https://webpack.js.org/',
    description: '静态模块打包工具，用于构建现代JavaScript应用程序',
    categoryId: 'tools',
    logo: '📦',
  },
  {
    id: 'vite',
    title: 'Vite',
    url: 'https://vitejs.dev/',
    description: '下一代前端构建工具，显著提高开发体验',
    categoryId: 'tools',
    logo: '⚡',
  },
  {
    id: 'git',
    title: 'Git',
    url: 'https://git-scm.com/',
    description: '分布式版本控制系统，用于跟踪文件的变化',
    categoryId: 'tools',
    logo: '📊',
  },

  // 文档教程
  {
    id: 'mdn',
    title: 'MDN Web Docs',
    url: 'https://developer.mozilla.org/',
    description: '权威的Web技术文档，包括HTML、CSS和JavaScript',
    categoryId: 'docs',
    logo: '📚',
  },
  {
    id: 'juejin',
    title: '掘金',
    url: 'https://juejin.cn/',
    description: '一个帮助开发者成长的社区，提供优质的技术内容',
    categoryId: 'docs',
    logo: '📖',
  },
  {
    id: 'ruanyf',
    title: '阮一峰的网络日志',
    url: 'https://www.ruanyifeng.com/blog/',
    description: '著名技术博客，涵盖广泛的前端和编程知识',
    categoryId: 'docs',
    logo: '🔍',
  },
  {
    id: 'w3schools',
    title: 'W3Schools',
    url: 'https://www.w3schools.com/',
    description: '最大的Web开发者网站，提供免费的教程和参考资料',
    categoryId: 'docs',
    logo: '🌐',
  },

  // 社区论坛
  {
    id: 'github',
    title: 'GitHub',
    url: 'https://github.com/',
    description: '面向开源及私有软件项目的托管平台',
    categoryId: 'community',
    logo: '🐙',
  },
  {
    id: 'stackoverflow',
    title: 'Stack Overflow',
    url: 'https://stackoverflow.com/',
    description: '程序设计领域的问答网站，技术问题解决的首选地',
    categoryId: 'community',
    logo: '❓',
  },
  {
    id: 'v2ex',
    title: 'V2EX',
    url: 'https://v2ex.com/',
    description: '创意工作者的社区，关注编程、设计和分享',
    categoryId: 'community',
    logo: '💬',
  },
  {
    id: 'segmentfault',
    title: 'SegmentFault',
    url: 'https://segmentfault.com/',
    description: '中国领先的开发者技术社区',
    categoryId: 'community',
    logo: '🔧',
  },

  // 设计资源
  {
    id: 'dribbble',
    title: 'Dribbble',
    url: 'https://dribbble.com/',
    description: '设计师展示作品和寻找灵感的平台',
    categoryId: 'design',
    logo: '🎨',
  },
  {
    id: 'behance',
    title: 'Behance',
    url: 'https://www.behance.net/',
    description: '创意专业人士的在线平台，展示和发现创意作品',
    categoryId: 'design',
    logo: '🖌️',
  },
  {
    id: 'figma',
    title: 'Figma',
    url: 'https://www.figma.com/',
    description: '基于浏览器的协作界面设计工具',
    categoryId: 'design',
    logo: '🎭',
  },
  {
    id: 'iconfont',
    title: 'Iconfont',
    url: 'https://www.iconfont.cn/',
    description: '阿里巴巴矢量图标库，提供海量免费图标',
    categoryId: 'design',
    logo: '🏷️',
  },
])

// 搜索功能
const searchQuery = ref('')
const selectedCategory = ref('all')

const filteredWebsites = computed(() => {
  let filtered = websites.value

  // 按分类筛选
  if (selectedCategory.value !== 'all') {
    filtered = filtered.filter((site) => site.categoryId === selectedCategory.value)
  }

  // 按搜索关键词筛选
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(
      (site) =>
        site.title.toLowerCase().includes(query) || site.description.toLowerCase().includes(query),
    )
  }

  return filtered
})

const getWebsitesByCategory = (categoryId: string) => {
  return websites.value.filter((site) => site.categoryId === categoryId)
}

// Icon组件
const getIcon = (iconName: string) => {
  switch (iconName) {
    case 'code':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M16 18l6-6-6-6M8 6l-6 6 6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
    case 'palette':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10c.926 0 1.648-.746 1.648-1.688 0-.437-.18-.835-.437-1.125-.29-.289-.438-.652-.438-1.125a1.64 1.64 0 011.668-1.668h1.996c3.051 0 5.555-2.503 5.555-5.554C21.002 6.012 17.022 2 12.002 2z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M7.5 13a1 1 0 100-2 1 1 0 000 2zM15.5 7a1 1 0 100-2 1 1 0 000 2zM18.5 13a1 1 0 100-2 1 1 0 000 2zM11.5 7a1 1 0 100-2 1 1 0 000 2zM7.5 7a1 1 0 100-2 1 1 0 000 2z" fill="currentColor"/>
            </svg>`
    case 'tool':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M14.7 6.3a1 1 0 000 1.4l1.6 1.6a1 1 0 001.4 0l3.77-3.77a6 6 0 01-7.94 7.94l-6.91 6.91a2.12 2.12 0 01-3-3l6.91-6.91a6 6 0 017.94-7.94l-3.76 3.76z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
    case 'book':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M4 19.5A2.5 2.5 0 016.5 17H20M4 19.5A2.5 2.5 0 016.5 22H20M4 19.5V5a2 2 0 012-2h12a2 2 0 012 2v14a2 2 0 01-2 2H6.5A2.5 2.5 0 014 19.5z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M12 7v6m-3-3h6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
    case 'users':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2M9 11a4 4 0 100-8 4 4 0 000 8zM23 21v-2a4 4 0 00-3-3.87M16 3.13a4 4 0 010 7.75" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
    case 'image':
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M19 3H5a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2V5a2 2 0 00-2-2z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M8.5 10a1.5 1.5 0 100-3 1.5 1.5 0 000 3zM21 15l-5-5L5 21" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
    default:
      return `<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 22c5.523 0 10-4.477 10-10S17.523 2 12 2 2 6.477 2 12s4.477 10 10 10z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>`
  }
}

const openWebsite = (url: string) => {
  window.open(url, '_blank')
}
</script>

<template>
  <div class="navigation-view">
    <section class="nav-header">
      <h1 class="nav-title">前端资源导航</h1>
      <p class="nav-subtitle">精选优质前端开发资源，助力您的开发效率</p>

      <div class="search-container">
        <div class="search-bar">
          <svg
            class="search-icon"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11 19a8 8 0 100-16 8 8 0 000 16zM21 21l-4.35-4.35"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          <input
            type="text"
            v-model="searchQuery"
            placeholder="搜索网站、关键词..."
            class="search-input"
          />
        </div>

        <div class="category-filter">
          <button
            class="category-btn"
            :class="{ active: selectedCategory === 'all' }"
            @click="selectedCategory = 'all'"
          >
            全部
          </button>
          <button
            v-for="category in categories"
            :key="category.id"
            class="category-btn"
            :class="{ active: selectedCategory === category.id }"
            @click="selectedCategory = category.id"
          >
            {{ category.name }}
          </button>
        </div>
      </div>
    </section>

    <section class="website-section">
      <div v-if="selectedCategory === 'all' && !searchQuery">
        <div v-for="category in categories" :key="category.id" class="category-section">
          <div class="category-header">
            <div class="category-icon" v-html="getIcon(category.icon)"></div>
            <h2 class="category-title">{{ category.name }}</h2>
          </div>

          <div class="websites-grid">
            <div
              v-for="website in getWebsitesByCategory(category.id)"
              :key="website.id"
              class="website-card"
              @click="openWebsite(website.url)"
            >
              <div class="website-logo">{{ website.logo }}</div>
              <div class="website-info">
                <h3 class="website-title">{{ website.title }}</h3>
                <p class="website-description">{{ website.description }}</p>
              </div>
              <div class="website-arrow">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M5 12h14M12 5l7 7-7 7"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else>
        <div class="filtered-results">
          <h2 class="results-title">
            {{
              filteredWebsites.length > 0
                ? `找到 ${filteredWebsites.length} 个结果`
                : '没有找到匹配的网站'
            }}
          </h2>

          <div class="websites-grid">
            <div
              v-for="website in filteredWebsites"
              :key="website.id"
              class="website-card"
              @click="openWebsite(website.url)"
            >
              <div class="website-logo">{{ website.logo }}</div>
              <div class="website-info">
                <h3 class="website-title">{{ website.title }}</h3>
                <p class="website-description">{{ website.description }}</p>
              </div>
              <div class="website-arrow">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M5 12h14M12 5l7 7-7 7"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.navigation-view {
  padding-bottom: 4rem;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.nav-header {
  text-align: center;
  margin-bottom: 3rem;
  width: 100%;
}

.nav-title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  background: linear-gradient(45deg, var(--color-primary), var(--color-accent));
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.nav-subtitle {
  font-size: 1.1rem;
  color: var(--color-text);
  margin-bottom: 2rem;
}

.search-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 0 1rem;
}

.search-bar {
  display: flex;
  align-items: center;
  background-color: var(--color-background-soft);
  border: 2px solid var(--color-border);
  border-radius: 50px;
  padding: 0.8rem 1.5rem;
  margin-bottom: 1.5rem;
  transition: all 0.3s ease;
  width: 100%;
}

.search-bar:focus-within {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
}

.search-icon {
  width: 20px;
  height: 20px;
  color: var(--color-text);
  margin-right: 0.8rem;
}

.search-input {
  flex: 1;
  background: none;
  border: none;
  font-size: 1rem;
  color: var(--color-text);
  outline: none;
}

.search-input::placeholder {
  color: var(--color-text-light-2);
}

.category-filter {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0.8rem;
  margin-bottom: 2rem;
}

.category-btn {
  background-color: var(--color-background-soft);
  color: var(--color-text);
  border: 1px solid var(--color-border);
  border-radius: 20px;
  padding: 0.5rem 1rem;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.category-btn:hover {
  background-color: var(--color-background-mute);
  border-color: var(--color-border-hover);
}

.category-btn.active {
  background-color: var(--color-primary);
  color: white;
  border-color: var(--color-primary);
}

.category-section {
  margin-bottom: 3rem;
  width: 100%;
}

.category-header {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
}

.category-icon {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--color-background-mute);
  border-radius: 8px;
  padding: 0.5rem;
  margin-right: 1rem;
  color: var(--color-primary);
}

.category-icon svg {
  width: 24px;
  height: 24px;
}

.category-title {
  font-size: 1.5rem;
  color: var(--color-heading);
}

.websites-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
  width: 100%;
}

.website-card {
  background-color: var(--color-background-soft);
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  cursor: pointer;
  border: 1px solid var(--color-border);
  height: 100%;
}

.website-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  border-color: var(--color-primary-soft);
}

.website-logo {
  font-size: 2rem;
  margin-right: 1rem;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.website-info {
  flex: 1;
}

.website-title {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: var(--color-heading);
}

.website-description {
  font-size: 0.9rem;
  color: var(--color-text);
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
}

.website-arrow {
  width: 24px;
  height: 24px;
  color: var(--color-primary);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.website-card:hover .website-arrow {
  opacity: 1;
}

.filtered-results {
  animation: fadeIn 0.5s ease;
}

.results-title {
  margin-bottom: 2rem;
  font-size: 1.5rem;
  color: var(--color-heading);
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@media (max-width: 768px) {
  .nav-title {
    font-size: 2rem;
  }

  .category-filter {
    gap: 0.5rem;
  }

  .category-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }

  .websites-grid {
    grid-template-columns: 1fr;
  }
}

.website-section {
  width: 100%;
}
</style>
