<script setup lang="ts">
import { ref } from 'vue'

interface Skill {
  name: string
  level: number // 1-10
  category: 'frontend' | 'backend' | 'other'
}

interface Project {
  title: string
  description: string
  tags: string[]
  image?: string
  link?: string
}

const profileData = ref({
  name: '雁翎',
  avatar: '/name.png',
  title: '前端学习者',
  bio: '热爱编程与技术，专注于创建美观且高性能的前端应用。喜欢学习新技术，并热衷于开源项目贡献。',
  location: '中国',
  email: '2779330253@qq.com',
  github: 'https://github.com/yanlin18',
  skills: [
    { name: 'HTML', level: 7, category: 'frontend' as const },
    { name: 'CSS', level: 7, category: 'frontend' as const },
    { name: 'JavaScript', level: 7, category: 'frontend' as const },
    { name: 'TypeScript', level: 3, category: 'frontend' as const },
    { name: 'Vue.js', level: 7, category: 'frontend' as const },
    { name: 'React', level: 2, category: 'frontend' as const },
    { name: 'Node.js', level: 1, category: 'backend' as const },
    { name: 'Express', level: 0, category: 'backend' as const },
    { name: 'MongoDB', level: 0, category: 'backend' as const },
    { name: 'Git', level: 7, category: 'other' as const },
    { name: 'UI/UX', level: 6, category: 'other' as const },
  ],
  projects: [
    {
      title: '仿抖音视频网站',
      description:
        '视频分享平台，允许用户上传、观看、分享和评论视频，采用Vue3和Element Plus构建。',
      tags: ['Vue3', 'Element Plus'],
      image: '/item/item1.png',
    },
    {
      title: '静态个人网站',
      description:
        '基于Vue3和TypeScript构建的个人静态资源网站，存放部分网站导航和个人资料，具有暗黑模式和音乐播放功能。',
      tags: ['Vue3', 'TypeScript'],
      image: '/item/item2.png',
    },
  ],
})

const frontendSkills = ref(
  profileData.value.skills.filter((skill) => skill.category === 'frontend'),
)
const backendSkills = ref(profileData.value.skills.filter((skill) => skill.category === 'backend'))
const otherSkills = ref(profileData.value.skills.filter((skill) => skill.category === 'other'))

const contactFormData = ref({
  name: '',
  email: '',
  message: '',
})

const showSuccessMessage = ref(false)

const handleSubmit = () => {
  // 这里可以添加表单提交逻辑，如API调用等
  console.log('Form submitted:', contactFormData.value)
  showSuccessMessage.value = true

  // 重置表单
  contactFormData.value = {
    name: '',
    email: '',
    message: '',
  }

  // 3秒后隐藏成功消息
  setTimeout(() => {
    showSuccessMessage.value = false
  }, 3000)
}
</script>

<template>
  <div class="profile-view">
    <section class="profile-header">
      <div class="profile-card">
        <div class="profile-avatar-container">
          <div class="profile-avatar">
            <img :src=profileData.avatar alt="Profile Avatar" />
          </div>
        </div>
        <div class="profile-info">
          <h1 class="profile-name">{{ profileData.name }}</h1>
          <p class="profile-title">{{ profileData.title }}</p>
          <p class="profile-bio">{{ profileData.bio }}</p>

          <div class="profile-contact">
            <div class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
                <circle
                  cx="12"
                  cy="10"
                  r="3"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              <span>{{ profileData.location }}</span>
            </div>

            <div class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
                <path
                  d="M22 6l-10 7L2 6"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              <span>{{ profileData.email }}</span>
            </div>
          </div>

          <div class="profile-social">
            <a :href="profileData.github" target="_blank" class="social-link">
              <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 00-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0020 4.77 5.07 5.07 0 0019.91 1S18.73.65 16 2.48a13.38 13.38 0 00-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 005 4.77a5.44 5.44 0 00-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 009 18.13V22"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </a>
          </div>
        </div>
      </div>
    </section>

    <div class="profile-content">
      <section class="skills-section">
        <h2 class="section-title">技能专长</h2>

        <div class="skills-container">
          <div class="skills-category">
            <h3 class="category-title">前端开发</h3>
            <div class="skills-list">
              <div v-for="skill in frontendSkills" :key="skill.name" class="skill-item">
                <div class="skill-info">
                  <span class="skill-name">{{ skill.name }}</span>
                  <span class="skill-level">{{ skill.level }}/10</span>
                </div>
                <div class="skill-bar">
                  <div class="skill-progress" :style="{ width: skill.level * 10 + '%' }"></div>
                </div>
              </div>
            </div>
          </div>

          <div class="skills-category">
            <h3 class="category-title">后端开发</h3>
            <div class="skills-list">
              <div v-for="skill in backendSkills" :key="skill.name" class="skill-item">
                <div class="skill-info">
                  <span class="skill-name">{{ skill.name }}</span>
                  <span class="skill-level">{{ skill.level }}/10</span>
                </div>
                <div class="skill-bar">
                  <div class="skill-progress" :style="{ width: skill.level * 10 + '%' }"></div>
                </div>
              </div>
            </div>
          </div>

          <div class="skills-category">
            <h3 class="category-title">其他技能</h3>
            <div class="skills-list">
              <div v-for="skill in otherSkills" :key="skill.name" class="skill-item">
                <div class="skill-info">
                  <span class="skill-name">{{ skill.name }}</span>
                  <span class="skill-level">{{ skill.level }}/10</span>
                </div>
                <div class="skill-bar">
                  <div class="skill-progress" :style="{ width: skill.level * 10 + '%' }"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="projects-section">
        <h2 class="section-title">个人项目</h2>

        <div class="projects-grid">
          <div v-for="project in profileData.projects" :key="project.title" class="project-card">
            <div class="project-image">
              <img :src="project.image" :alt="project.title" />
            </div>
            <div class="project-content">
              <h3 class="project-title">{{ project.title }}</h3>
              <p class="project-description">{{ project.description }}</p>
              <div class="project-tags">
                <span v-for="(tag, index) in project.tags" :key="index" class="project-tag">{{
                  tag
                }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>

  </div>
</template>

<style scoped>
.profile-view {
  padding-bottom: 4rem;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.profile-header {
  margin-bottom: 3rem;
  width: 100%;
}

.profile-card {
  background-color: var(--color-background-soft);
  border-radius: 20px;
  padding: 2rem;
  display: flex;
  gap: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  border: 1px solid var(--color-border);
  position: relative;
  overflow: hidden;
  margin: 0 auto;
  width: 100%;
  flex-direction: column;
  align-items: center;
}

.profile-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, var(--color-primary), var(--color-accent));
  opacity: 0.05;
  z-index: 0;
}

.profile-avatar-container {
  position: relative;
  z-index: 1;
}

.profile-avatar {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid var(--color-primary);
  position: relative;
}

.profile-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.profile-info {
  flex: 1;
  position: relative;
  z-index: 1;
}

.profile-name {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: var(--color-heading);
}

.profile-title {
  font-size: 1.2rem;
  color: var(--color-primary);
  margin-bottom: 1rem;
  font-weight: 500;
}

.profile-bio {
  margin-bottom: 1.5rem;
  line-height: 1.6;
  color: var(--color-text);
}

.profile-contact {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.contact-item svg {
  width: 18px;
  height: 18px;
  color: var(--color-primary);
}

.profile-social {
  display: flex;
  gap: 1rem;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: var(--color-background-mute);
  color: var(--color-primary);
  transition: all 0.3s ease;
}

.social-link:hover {
  background-color: var(--color-primary);
  color: white;
  transform: translateY(-3px);
}

.social-link svg {
  width: 18px;
  height: 18px;
}

.section-title {
  font-size: 1.8rem;
  margin-bottom: 2rem;
  color: var(--color-heading);
  position: relative;
  padding-bottom: 0.5rem;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 50px;
  height: 3px;
  background: linear-gradient(45deg, var(--color-primary), var(--color-accent));
}

.profile-content {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  margin: 0 auto;
  width: 100%;
}

.skills-section,
.projects-section {
  margin-bottom: 3rem;
  width: 100%;
}

@media (min-width: 992px) {
  .skills-section {
    flex: 1;
    min-width: 300px;
  }

  .projects-section {
    flex: 2;
    min-width: 600px;
  }
}

.skills-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
}

.category-title {
  font-size: 1.3rem;
  margin-bottom: 1.5rem;
  color: var(--color-primary);
  font-weight: 600;
}

.skills-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.skill-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.skill-info {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
}

.skill-name {
  font-weight: 500;
  color: var(--color-heading);
}

.skill-level {
  color: var(--color-text);
}

.skill-bar {
  height: 8px;
  background-color: var(--color-background-mute);
  border-radius: 4px;
  overflow: hidden;
}

.skill-progress {
  height: 100%;
  background: linear-gradient(45deg, var(--color-primary), var(--color-primary-soft));
  border-radius: 4px;
  transition: width 1s ease;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
}

.project-card {
  background-color: var(--color-background-soft);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  border: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  height: 100%;
}

.project-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
  border-color: var(--color-primary-soft);
}

.project-image {
  height: 180px;
  overflow: hidden;
}

.project-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.project-card:hover .project-image img {
  transform: scale(1.05);
}

.project-content {
  padding: 1.5rem;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.project-title {
  font-size: 1.2rem;
  margin-bottom: 1rem;
  color: var(--color-heading);
  font-weight: 600;
}

.project-description {
  margin-bottom: 1.5rem;
  color: var(--color-text);
  line-height: 1.6;
  flex: 1;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.project-tag {
  font-size: 0.8rem;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  background-color: var(--color-background-mute);
  color: var(--color-primary);
}

.contact-container {
  max-width: 600px;
  margin: 0 auto;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--color-heading);
}

.form-group input,
.form-group textarea {
  padding: 0.8rem 1rem;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  background-color: var(--color-background-soft);
  color: var(--color-text);
  transition: all 0.3s ease;
}

.form-group textarea {
  min-height: 150px;
  resize: vertical;
}

.form-group input:focus,
.form-group textarea:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
  outline: none;
}

.submit-btn {
  padding: 0.8rem 1.5rem;
  border-radius: 8px;
  background: linear-gradient(45deg, var(--color-primary), var(--color-primary-soft));
  color: white;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  align-self: flex-start;
}

.submit-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 15px rgba(66, 184, 131, 0.4);
}

.success-message {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background-color: rgba(66, 184, 131, 0.1);
  color: var(--color-primary);
  padding: 1rem;
  border-radius: 8px;
  animation: fadeIn 0.5s ease;
}

.success-message svg {
  width: 20px;
  height: 20px;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@media (max-width: 1200px) {
  .profile-content {
    flex-direction: column;
  }

  .skills-section,
  .projects-section {
    width: 100%;
  }
}

@media (max-width: 768px) {
  .profile-card {
    flex-direction: column;
    align-items: center;
    padding: 1.5rem;
    gap: 1.5rem;
  }

  .profile-avatar {
    width: 120px;
    height: 120px;
  }

  .profile-name {
    font-size: 1.5rem;
    text-align: center;
  }

  .profile-title {
    font-size: 1rem;
    text-align: center;
  }

  .profile-contact {
    flex-direction: column;
    gap: 1rem;
  }

  .profile-social {
    justify-content: center;
  }
}

@media (min-width: 992px) {
  .profile-card {
    flex-direction: row;
    align-items: flex-start;
  }
}

.contact-section {
  width: 100%;
}
</style>
