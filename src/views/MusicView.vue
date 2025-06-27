<script setup lang="ts">
import { ref, computed, onMounted, watchEffect } from 'vue'

interface Song {
  id: string
  title: string
  artist: string
  cover: string
  url: string
  duration: number
}

// 歌曲数据
const songs = ref<Song[]>([
  {
    id: '1',
    title: '',
    artist: '本地音乐库',
    cover: '/musicphotos/musicphoto1.jpg',
    url: '/audio/music1.mp3',
    duration: 0, // 将动态获取
  },
  {
    id: '2',
    title: '春庭雪',
    artist: '本地音乐库',
    cover: '/musicphotos/musicphoto2.jpg',
    url: '/audio/music2.mp3',
    duration: 0, // 将动态获取
  },
  {
    id: '3',
    title: '千年之恋',
    artist: '本地音乐库',
    cover: '/musicphotos/musicphoto3.jpg',
    url: '/audio/music3.mp3',
    duration: 0, // 将动态获取
  },
  {
    id: '4',
    title: '霜雪千年 ',
    artist: '本地音乐库',
    cover: '/musicphotos/musicphoto4.jpg',
    url: '/audio/music4.mp3',
    duration: 0, // 将动态获取
  },
  {
    id: '5',
    title: '燕无歇',
    artist: '本地音乐库',
    cover: '/musicphotos/musicphoto5.jpg',
    url: '/audio/music5.mp3',
    duration: 0, // 将动态获取
  },
])

// 播放器状态
const isPlaying = ref(false)
const currentSongIndex = ref(0)
const currentTime = ref(0)
const audioElement = ref<HTMLAudioElement | null>(null)
const progressBarRef = ref<HTMLElement | null>(null)
const visualizerRef = ref<HTMLElement | null>(null)
const audioContext = ref<AudioContext | null>(null)
const analyzer = ref<AnalyserNode | null>(null)
const isVisualizing = ref(false)

// 计算属性
const currentSong = computed(() => songs.value[currentSongIndex.value])
const formatTime = (seconds: number) => {
  if (isNaN(seconds) || seconds === 0) {
    return '0:00'
  }

  const mins = Math.floor(seconds / 60)
  const secs = Math.floor(seconds % 60)
  return `${mins}:${secs < 10 ? '0' : ''}${secs}`
}

const formattedCurrentTime = computed(() => formatTime(currentTime.value))
const formattedDuration = computed(() => formatTime(currentSong.value.duration))
const progressPercentage = computed(() => (currentTime.value / currentSong.value.duration) * 100)

// 生命周期钩子
onMounted(() => {
  // 确保在用户交互后创建音频元素，避免自动播放限制
  setTimeout(() => {
    // 直接创建新的音频元素
    audioElement.value = new Audio()

    // 设置音频属性
    audioElement.value.preload = 'auto'
    audioElement.value.volume = 1.0 // 确保音量是最大的
    audioElement.value.crossOrigin = 'anonymous' // 允许跨域请求

    // 设置初始歌曲
    setAudioSource(currentSong.value.url)

    // 添加事件监听
    audioElement.value.addEventListener('timeupdate', updateProgress)
    audioElement.value.addEventListener('ended', playNext)
    audioElement.value.addEventListener('error', handleAudioError)

    // 监听当前歌曲变化
    watchEffect(() => {
      if (audioElement.value && currentSong.value) {
        setAudioSource(currentSong.value.url)
        currentTime.value = 0
      }
    })

    // 添加默认背景图案
    if (visualizerRef.value) {
      drawDefaultPattern()
    }

    // 预加载所有歌曲的时长信息
    preloadAllSongsDuration()

    // 初始加载测试音频
    console.log('正在加载音频:', currentSong.value.url)
  }, 100)
})

// 预加载所有歌曲的时长信息
const preloadAllSongsDuration = () => {
  songs.value.forEach((song, index) => {
    if (song.duration === 0) {
      const tempAudio = new Audio()
      tempAudio.src = song.url

      // 监听元数据加载事件
      tempAudio.addEventListener('loadedmetadata', () => {
        if (!isNaN(tempAudio.duration)) {
          // 更新歌曲时长
          songs.value[index].duration = tempAudio.duration
          console.log(`歌曲 ${song.title} 时长加载完成: ${tempAudio.duration}秒`)
        }
      })

      // 监听错误事件
      tempAudio.addEventListener('error', () => {
        console.error(`无法加载歌曲 ${song.title} 的时长信息`)

        // 使用备用音频尝试加载时长
        const backupAudio = new Audio()
        // 尝试使用本地备用曲目
        const fallbackUrl = `/audio/song${index + 1}.mp3`
        backupAudio.src = fallbackUrl

        backupAudio.addEventListener('loadedmetadata', () => {
          if (!isNaN(backupAudio.duration)) {
            songs.value[index].duration = backupAudio.duration
            console.log(`使用备用源加载歌曲 ${song.title} 时长: ${backupAudio.duration}秒`)
          }
        })

        // 加载备用源
        backupAudio.load()
      })

      // 开始加载
      tempAudio.load()
    }
  })
}

// 添加音频错误处理函数
const handleAudioError = (e: Event) => {
  console.error('音频加载错误:', e)
  console.log('尝试使用备用音频源')

  // 尝试使用内部函数处理错误
  tryFallbackAudio()
}

// 备用音频加载
const tryFallbackAudio = () => {
  // 备用音频文件URL列表 - 使用本地音频
  const fallbackUrls = [
    // 本地备用音频
    '/audio/song1.mp3',
    '/audio/song2.mp3',
    '/audio/song3.mp3',
    '/audio/song4.mp3',
    '/audio/song5.mp3',
  ]

  // 随机选择一个备用URL
  const randomFallback = fallbackUrls[Math.floor(Math.random() * fallbackUrls.length)]

  // 设置备用源
  if (audioElement.value) {
    console.log('使用备用音频源:', randomFallback)
    setAudioSource(randomFallback)

    // 尝试播放备用音频
    setTimeout(() => {
      if (audioElement.value) {
        const audioEl = audioElement.value
        audioEl
          .play()
          .then(() => {
            console.log('备用音频播放成功')
            isPlaying.value = true

            // 通知用户
            alert('已切换到本地备用音频')

            // 播放时检查是否需要启动可视化效果
            if (!isVisualizing.value && visualizerRef.value) {
              toggleVisualizer()
            }
          })
          .catch((error) => {
            console.error('备用音频播放失败:', error)
            isPlaying.value = false
            alert('所有音频资源都无法播放，请检查浏览器设置或网络连接')
          })
      }
    }, 500)
  }
}

// 设置音频源的统一函数
const setAudioSource = (src: string) => {
  if (!audioElement.value) return

  try {
    // 移除之前的错误监听器以避免重复处理
    audioElement.value.removeEventListener('error', handleAudioError)

    audioElement.value.pause()
    audioElement.value.src = src
    audioElement.value.load()

    // 添加事件监听器获取音频时长
    const loadMetadataHandler = () => {
      if (audioElement.value && !isNaN(audioElement.value.duration)) {
        // 更新当前歌曲的持续时间
        songs.value[currentSongIndex.value].duration = audioElement.value.duration
        // 移除监听器避免重复调用
        audioElement.value.removeEventListener('loadedmetadata', loadMetadataHandler)
      }
    }

    audioElement.value.addEventListener('loadedmetadata', loadMetadataHandler)

    // 重新添加错误监听器
    audioElement.value.addEventListener('error', handleAudioError)

    // 记录当前尝试的URL
    console.log('设置音频源:', src)
  } catch (error) {
    console.error('设置音频源失败:', error)
  }
}

// 播放控制方法
const togglePlay = () => {
  if (!audioElement.value) return

  try {
    if (isPlaying.value) {
      // 暂停
      audioElement.value.pause()
      isPlaying.value = false

      // 如果正在可视化，也暂停可视化
      if (isVisualizing.value) {
        toggleVisualizer()
      }
    } else {
      // 尝试播放
      console.log('尝试播放音频:', audioElement.value.src)

      // 恢复音频上下文（如果存在）
      if (audioContext.value && audioContext.value.state === 'suspended') {
        audioContext.value.resume().catch((e) => console.error('恢复音频上下文失败:', e))
      }

      const audioEl = audioElement.value
      if (!audioEl) return

      audioEl
        .play()
        .then(() => {
          console.log('音频播放成功')
          isPlaying.value = true

          // 播放时检查是否需要启动可视化效果
          if (!isVisualizing.value && visualizerRef.value) {
            toggleVisualizer()
          }
        })
        .catch((error) => {
          console.error('音频播放失败:', error)

          if (!audioEl) return

          // 可能是自动播放策略阻止，尝试静音播放然后取消静音
          audioEl.muted = true
          audioEl
            .play()
            .then(() => {
              // 成功启动播放，0.5秒后取消静音
              setTimeout(() => {
                if (audioEl) {
                  audioEl.muted = false
                  isPlaying.value = true

                  // 播放时检查是否需要启动可视化效果
                  if (!isVisualizing.value && visualizerRef.value) {
                    toggleVisualizer()
                  }
                }
              }, 500)
            })
            .catch((innerError) => {
              console.error('静音播放也失败:', innerError)
              isPlaying.value = false
              alert('播放失败，浏览器可能阻止了自动播放')
            })
        })
    }
  } catch (error) {
    console.error('播放控制错误:', error)
    isPlaying.value = false
  }
}

const playNext = () => {
  currentSongIndex.value = (currentSongIndex.value + 1) % songs.value.length
  if (isPlaying.value) {
    setTimeout(() => {
      audioElement.value?.play()
    }, 10)
  }
}

const playPrev = () => {
  currentSongIndex.value = (currentSongIndex.value - 1 + songs.value.length) % songs.value.length
  if (isPlaying.value) {
    setTimeout(() => {
      audioElement.value?.play()
    }, 10)
  }
}

const updateProgress = (e: Event) => {
  if (!audioElement.value) return
  // 确保当前时间不会超过音频总时长
  currentTime.value = Math.min(audioElement.value.currentTime, audioElement.value.duration || 0)
}

const setProgress = (e: MouseEvent) => {
  if (!progressBarRef.value || !audioElement.value) return

  const width = progressBarRef.value.clientWidth
  const clickX = e.offsetX
  const duration = audioElement.value.duration || currentSong.value.duration

  // 确保新的当前时间在有效范围内
  const newTime = (clickX / width) * duration
  audioElement.value.currentTime = Math.max(0, Math.min(newTime, duration))
}

// 可视化效果
const toggleVisualizer = () => {
  try {
    isVisualizing.value = !isVisualizing.value

    if (isVisualizing.value) {
      setupVisualizer()

      // 视觉反馈
      if (visualizerRef.value) {
        visualizerRef.value.classList.add('active-visualizer')
      }
    } else {
      cancelAnimationFrame(animationId.value)
      drawDefaultPattern() // 恢复默认图案

      // 移除视觉反馈
      if (visualizerRef.value) {
        visualizerRef.value.classList.remove('active-visualizer')
      }
    }
  } catch (error) {
    console.error('切换可视化效果失败:', error)
    isVisualizing.value = false
    drawDefaultPattern()
  }
}

let animationId = ref(0)

// 修改setupVisualizer，简化音频连接和处理逻辑
const setupVisualizer = () => {
  if (!audioElement.value || !visualizerRef.value) return

  try {
    // 确保音频上下文正确初始化
    if (!audioContext.value) {
      // 创建新的音频上下文
      audioContext.value = new (window.AudioContext || (window as any).webkitAudioContext)()
    }

    // 确保分析器节点存在
    if (!analyzer.value) {
      analyzer.value = audioContext.value.createAnalyser()
      analyzer.value.fftSize = 512 // 增加分析精度，提供更多数据点

      // 创建一个新的媒体源（只应该创建一次）
      try {
        const source = audioContext.value.createMediaElementSource(audioElement.value)
        source.connect(analyzer.value)
        analyzer.value.connect(audioContext.value.destination)
      } catch (e) {
        console.log('媒体源可能已经连接，继续使用现有连接')
      }
    }

    const bufferLength = analyzer.value.frequencyBinCount
    const dataArray = new Uint8Array(bufferLength)

    // 创建新的canvas
    const canvas = document.createElement('canvas')
    canvas.width = visualizerRef.value.clientWidth
    canvas.height = visualizerRef.value.clientHeight
    visualizerRef.value.innerHTML = ''
    visualizerRef.value.appendChild(canvas)

    const ctx = canvas.getContext('2d')!

    // 创建多个渐变色
    const gradient1 = ctx.createLinearGradient(0, 0, canvas.width, 0)
    gradient1.addColorStop(0, 'rgba(66, 184, 131, 0.8)') // 绿色
    gradient1.addColorStop(1, 'rgba(66, 184, 131, 0.2)')

    const gradient2 = ctx.createLinearGradient(0, 0, canvas.width, 0)
    gradient2.addColorStop(0, 'rgba(255, 126, 95, 0.8)') // 橙色
    gradient2.addColorStop(1, 'rgba(255, 126, 95, 0.2)')

    const gradient3 = ctx.createLinearGradient(0, 0, canvas.width, 0)
    gradient3.addColorStop(0, 'rgba(125, 95, 255, 0.8)') // 紫色
    gradient3.addColorStop(1, 'rgba(125, 95, 255, 0.2)')

    const gradient4 = ctx.createLinearGradient(0, 0, canvas.width, 0)
    gradient4.addColorStop(0, 'rgba(255, 215, 0, 0.8)') // 金色
    gradient4.addColorStop(1, 'rgba(255, 215, 0, 0.2)')

    // 使用多种波形可视化
    const renderFrame = () => {
      if (!isVisualizing.value || !analyzer.value) return

      animationId.value = requestAnimationFrame(renderFrame)

      try {
        // 获取频率数据
        analyzer.value.getByteFrequencyData(dataArray)

        // 清空画布
        ctx.clearRect(0, 0, canvas.width, canvas.height)

        // 创建背景
        const bgGradient = ctx.createLinearGradient(0, 0, 0, canvas.height)
        bgGradient.addColorStop(0, 'rgba(25, 25, 35, 0.5)')
        bgGradient.addColorStop(1, 'rgba(10, 10, 20, 0.5)')
        ctx.fillStyle = bgGradient
        ctx.fillRect(0, 0, canvas.width, canvas.height)

        // 绘制第一条波形线 - 正弦波
        ctx.beginPath()
        ctx.moveTo(0, canvas.height / 2)

        // 平滑波形
        for (let i = 0; i < bufferLength; i++) {
          const value = dataArray[i] / 255
          const y = canvas.height / 2 + ((value * canvas.height) / 3) * Math.sin(i / 5)
          const x = (i / bufferLength) * canvas.width

          ctx.lineTo(x, y)
        }

        ctx.strokeStyle = gradient1
        ctx.lineWidth = 3
        ctx.stroke()

        // 绘制第二条波形线 - 余弦波
        ctx.beginPath()
        ctx.moveTo(0, canvas.height / 2)

        for (let i = 0; i < bufferLength; i++) {
          const value = dataArray[i] / 255
          const y = canvas.height / 2 - ((value * canvas.height) / 4) * Math.cos(i / 10)
          const x = (i / bufferLength) * canvas.width

          ctx.lineTo(x, y)
        }

        ctx.strokeStyle = gradient2
        ctx.lineWidth = 2
        ctx.stroke()

        // 绘制第三条波形线 - 正切波
        ctx.beginPath()
        ctx.moveTo(0, canvas.height / 2)

        for (let i = 0; i < bufferLength; i++) {
          const value = dataArray[i] / 255
          // 使用tan函数时需要限制范围以避免值过大
          const tanValue = Math.min(Math.max(Math.tan(i / 30) * 0.2, -1), 1)
          const y = canvas.height / 2 + ((value * canvas.height) / 5) * tanValue
          const x = (i / bufferLength) * canvas.width

          ctx.lineTo(x, y)
        }

        ctx.strokeStyle = gradient3
        ctx.lineWidth = 1.5
        ctx.stroke()

        // 绘制第四条波形线 - 点状波形
        for (let i = 0; i < bufferLength; i += 2) {
          const value = dataArray[i] / 255
          const y =
            canvas.height / 2 + ((value * canvas.height) / 2) * Math.sin(i / 20 + Date.now() / 1000)
          const x = (i / bufferLength) * canvas.width

          const radius = value * 3 + 1
          ctx.beginPath()
          ctx.arc(x, y, radius, 0, Math.PI * 2)
          ctx.fillStyle = gradient4
          ctx.fill()
        }

        // 绘制垂直频谱柱状图 - 底部
        const barWidth = canvas.width / (bufferLength / 4)
        let barHeight
        let x = 0

        for (let i = 0; i < bufferLength; i += 4) {
          barHeight = (dataArray[i] / 255) * (canvas.height / 4)

          // 创建每个柱的渐变
          const barGradient = ctx.createLinearGradient(
            0,
            canvas.height - barHeight,
            0,
            canvas.height,
          )
          barGradient.addColorStop(0, 'rgba(66, 184, 131, 0.7)')
          barGradient.addColorStop(1, 'rgba(66, 184, 131, 0.2)')

          ctx.fillStyle = barGradient
          ctx.fillRect(x, canvas.height - barHeight, barWidth - 1, barHeight)

          x += barWidth
        }
      } catch (error) {
        console.error('可视化渲染错误:', error)
        cancelAnimationFrame(animationId.value)
        drawDefaultPattern()
        isVisualizing.value = false
      }
    }

    renderFrame()
  } catch (error) {
    console.error('设置可视化效果失败:', error)
    drawDefaultPattern()
    isVisualizing.value = false
  }
}

// 为可视化区域添加默认图案，使用动态波浪
const drawDefaultPattern = () => {
  if (!visualizerRef.value) return

  const canvas = document.createElement('canvas')
  canvas.width = visualizerRef.value.clientWidth
  canvas.height = visualizerRef.value.clientHeight
  visualizerRef.value.innerHTML = ''
  visualizerRef.value.appendChild(canvas)

  const ctx = canvas.getContext('2d')!

  // 创建渐变背景
  const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height)
  gradient.addColorStop(0, 'rgba(25, 25, 35, 0.3)')
  gradient.addColorStop(1, 'rgba(10, 10, 20, 0.3)')

  // 动画函数和波浪参数
  let offset = 0

  const animate = () => {
    if (isVisualizing.value) return // 如果开始可视化就停止动画

    offset += 0.3

    // 清空画布但保留背景
    ctx.fillStyle = gradient
    ctx.fillRect(0, 0, canvas.width, canvas.height)

    // 绘制第一条静态波浪线
    ctx.beginPath()
    ctx.moveTo(0, canvas.height / 2)

    for (let x = 0; x < canvas.width; x += 5) {
      const y = canvas.height / 2 + Math.sin(x / 30 + offset) * 15
      ctx.lineTo(x, y)
    }

    ctx.strokeStyle = 'rgba(66, 184, 131, 0.5)'
    ctx.lineWidth = 2
    ctx.stroke()

    // 绘制第二条静态波浪线
    ctx.beginPath()
    ctx.moveTo(0, canvas.height / 2)

    for (let x = 0; x < canvas.width; x += 5) {
      const y = canvas.height / 2 + Math.cos(x / 20 - offset) * 10
      ctx.lineTo(x, y)
    }

    ctx.strokeStyle = 'rgba(255, 126, 95, 0.5)'
    ctx.lineWidth = 1.5
    ctx.stroke()

    // 绘制第三条静态波浪线
    ctx.beginPath()
    ctx.moveTo(0, canvas.height / 2)

    for (let x = 0; x < canvas.width; x += 5) {
      const y = canvas.height / 2 + Math.sin(x / 40 + offset * 0.7) * 20
      ctx.lineTo(x, y)
    }

    ctx.strokeStyle = 'rgba(125, 95, 255, 0.4)'
    ctx.lineWidth = 1.5
    ctx.stroke()

    // 绘制第四条静态波浪线 - 点状
    for (let x = 0; x < canvas.width; x += 15) {
      const y = canvas.height / 2 + Math.cos(x / 25 - offset * 0.5) * 12

      ctx.beginPath()
      ctx.arc(x, y, 2, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(255, 215, 0, 0.6)'
      ctx.fill()
    }

    // 添加提示文字
    const fontSize = Math.min(canvas.width / 20, 30)

    // 添加主标题
    ctx.fillStyle = 'rgba(66, 184, 131, 0.7)'
    ctx.font = `bold ${fontSize}px Arial`
    ctx.textAlign = 'center'
    ctx.textBaseline = 'middle'
    ctx.fillText('♫ 音乐可视化 ♫', canvas.width / 2, canvas.height / 2 - fontSize)

    // 添加提示文字
    ctx.fillStyle = 'rgba(66, 184, 131, 0.6)'
    ctx.font = `${fontSize * 0.5}px Arial`
    ctx.fillText('点击此区域开始可视化效果', canvas.width / 2, canvas.height / 2 + fontSize)

    // 继续动画
    if (!isVisualizing.value) {
      requestAnimationFrame(animate)
    }
  }

  // 开始默认动画
  animate()
}

// 选择歌曲
const selectSong = (index: number) => {
  // 更新当前歌曲索引
  currentSongIndex.value = index

  // 重置播放状态
  if (audioElement.value) {
    audioElement.value.pause()
    audioElement.value.currentTime = 0
    currentTime.value = 0
  }

  // 尝试播放新歌曲
  setTimeout(() => {
    try {
      if (!audioElement.value) return

      // 设置新的音频源
      setAudioSource(currentSong.value.url)

      // 恢复音频上下文
      if (audioContext.value && audioContext.value.state === 'suspended') {
        audioContext.value.resume().catch((e) => console.error('恢复音频上下文失败:', e))
      }

      const audioEl = audioElement.value
      if (!audioEl) return

      // 播放
      console.log('尝试播放新歌曲:', audioEl.src)
      audioEl
        .play()
        .then(() => {
          console.log('新歌曲播放成功')
          isPlaying.value = true

          // 播放时启动可视化效果
          if (!isVisualizing.value && visualizerRef.value) {
            toggleVisualizer()
          }
        })
        .catch((error) => {
          console.error('新歌曲播放失败:', error)

          if (!audioEl) return

          // 尝试静音播放
          audioEl.muted = true
          audioEl
            .play()
            .then(() => {
              // 成功启动播放，0.5秒后取消静音
              setTimeout(() => {
                if (audioEl) {
                  audioEl.muted = false
                  isPlaying.value = true

                  // 播放时检查是否需要启动可视化效果
                  if (!isVisualizing.value && visualizerRef.value) {
                    toggleVisualizer()
                  }
                }
              }, 500)
            })
            .catch((innerError) => {
              console.error('静音播放也失败:', innerError)
              isPlaying.value = false

              // 尝试备用音频
              tryFallbackAudio()
            })
        })
    } catch (error) {
      console.error('选择歌曲错误:', error)
      isPlaying.value = false
      tryFallbackAudio()
    }
  }, 100) // 给予足够的延迟确保DOM已更新
}
</script>

<template>
  <div class="music-view">
    <section class="music-header">
      <h1 class="music-title">音乐播放器</h1>
      <p class="music-subtitle">享受编程时的音乐时光</p>
    </section>

    <div class="music-player">
      <div class="player-info">
        <div class="current-track">
          <div class="track-artwork">
            <img :src="currentSong.cover" :alt="currentSong.title" />
          </div>
          <div class="track-details">
            <h2 class="track-title">{{ currentSong.title }}</h2>
            <p class="track-artist">{{ currentSong.artist }}</p>
            <p class="track-album">精选音乐</p>
          </div>
        </div>

        <div class="player-controls">
          <div class="progress-bar" ref="progressBarRef" @click="setProgress">
            <div class="progress-fill" :style="{ width: `${progressPercentage}%` }"></div>
          </div>
          <div class="progress-info">
            <span>{{ formattedCurrentTime }}</span>
            <span>{{ formattedDuration }}</span>
          </div>

          <div class="control-buttons">
            <button class="control-button" @click="playPrev">
              <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M19 20L9 12l10-8v16zM5 19V5"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
            <button class="control-button play-pause" @click="togglePlay">
              <svg
                v-if="!isPlaying"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path d="M5 3l14 9-14 9V3z" fill="currentColor" />
              </svg>
              <svg v-else viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M6 4h4v16H6V4zM14 4h4v16h-4V4z" fill="currentColor" />
              </svg>
            </button>
            <button class="control-button" @click="playNext">
              <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M5 4l10 8-10 8V4zM19 5v14"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>

      <div class="visualizer-container" ref="visualizerRef" @click="toggleVisualizer">
        <canvas class="visualizer-canvas"></canvas>
        <button class="visualizer-toggle" @click.stop="toggleVisualizer">
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M2 12h4M6 8v8M10 4v16M14 9v6M18 7v10M22 11v2"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </button>
      </div>
    </div>

    <section class="playlist">
      <h2 class="playlist-title">播放列表</h2>
      <div class="tracks-list">
        <div
          v-for="(song, index) in songs"
          :key="song.id"
          class="track-item"
          :class="{ active: currentSongIndex === index }"
          @click="selectSong(index)"
        >
          <div class="track-number">{{ index + 1 }}</div>
          <div class="track-item-artwork">
            <img :src="song.cover" :alt="song.title" />
          </div>
          <div class="track-item-info">
            <div class="track-item-title">{{ song.title }}</div>
            <div class="track-item-artist">{{ song.artist }}</div>
          </div>
          <div class="track-item-duration">{{ formatTime(song.duration) }}</div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.music-view {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
}

.music-header {
  text-align: center;
  margin-bottom: 3rem;
  width: 100%;
}

.music-title {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
  background: linear-gradient(135deg, var(--color-primary), var(--color-accent));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.music-subtitle {
  font-size: 1.2rem;
  color: var(--color-text-light);
}

.music-player {
  display: flex;
  gap: 2rem;
  margin-bottom: 3rem;
  padding: 2rem;
  background-color: var(--color-background-soft);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.player-info {
  flex: 1;
  min-width: 300px;
}

.current-track {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.track-artwork {
  width: 80px;
  height: 80px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.track-artwork img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.track-details {
  flex: 1;
}

.track-title {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-heading);
}

.track-artist {
  font-size: 1rem;
  margin-bottom: 0.5rem;
  color: var(--color-text);
}

.track-album {
  font-size: 0.9rem;
  color: var(--color-text);
}

.player-controls {
  margin-bottom: 1.5rem;
}

.progress-bar {
  width: 100%;
  height: 6px;
  background-color: rgba(var(--color-border-rgb), 0.5);
  border-radius: 3px;
  margin-bottom: 0.5rem;
  cursor: pointer;
  position: relative;
}

.progress-fill {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  background-color: var(--color-primary);
  border-radius: 3px;
  transition: width 0.1s linear;
}

.progress-info {
  display: flex;
  justify-content: space-between;
  font-size: 0.8rem;
  color: var(--color-text-light);
  margin-bottom: 1rem;
}

.control-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
}

.control-button {
  background: none;
  border: none;
  color: var(--color-text);
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.control-button:hover {
  color: var(--color-primary);
  transform: scale(1.1);
}

.control-button svg {
  width: 24px;
  height: 24px;
}

.play-pause {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: var(--color-background);
  color: var(--color-heading);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.play-pause:hover {
  background-color: var(--color-primary);
  color: white;
  transform: scale(1.05);
}

.play-pause svg {
  width: 28px;
  height: 28px;
}

.visualizer-container {
  flex: 2;
  height: 300px;
  background-color: var(--color-background-mute);
  border-radius: 12px;
  overflow: hidden;
  position: relative;
  transition: box-shadow 0.3s ease;
  cursor: pointer;
}

.active-visualizer {
  box-shadow: 0 0 20px rgba(66, 184, 131, 0.5);
}

.visualizer-canvas {
  width: 100%;
  height: 100%;
}

.visualizer-toggle {
  position: absolute;
  bottom: 10px;
  right: 10px;
  width: 36px;
  height: 36px;
  background-color: rgba(0, 0, 0, 0.7);
  border-radius: 50%;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
}

.visualizer-toggle:hover {
  background-color: var(--color-primary);
}

.visualizer-toggle svg {
  width: 20px;
  height: 20px;
}

.playlist {
  margin: 0 auto;
  width: 100%;
}

.playlist-title {
  font-size: 1.8rem;
  margin-bottom: 2rem;
  position: relative;
  display: inline-block;
}

.playlist-title::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 0;
  width: 40px;
  height: 3px;
  background-color: var(--color-primary);
  border-radius: 3px;
}

.tracks-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.track-item {
  display: flex;
  align-items: center;
  padding: 1rem;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  background-color: var(--color-background-soft);
  transition: all 0.3s ease;
  cursor: pointer;
  gap: 1rem;
}

.track-item:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  border-color: var(--color-primary-soft);
}

.track-item.active {
  border-color: var(--color-primary);
  background-color: rgba(var(--color-primary-rgb), 0.1);
}

.track-number {
  font-size: 0.9rem;
  color: var(--color-text);
  width: 20px;
  text-align: center;
}

.track-item-artwork {
  width: 50px;
  height: 50px;
  border-radius: 5px;
  overflow: hidden;
}

.track-item-artwork img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.track-item-info {
  flex: 1;
}

.track-item-title {
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 0.2rem;
  color: var(--color-heading);
}

.track-item-artist {
  font-size: 0.9rem;
  color: var(--color-text);
}

.track-item-duration {
  font-size: 0.9rem;
  color: var(--color-text);
}

@media (max-width: 992px) {
  .music-player {
    flex-direction: column;
  }

  .visualizer-container {
    height: 200px;
  }
}

@media (max-width: 768px) {
  .music-view {
    padding: 1rem;
  }

  .music-player {
    padding: 1.5rem;
  }

  .track-item {
    padding: 0.75rem;
  }
}

@media (max-width: 576px) {
  .music-title {
    font-size: 2rem;
  }

  .music-subtitle {
    font-size: 1rem;
  }

  .track-artwork {
    width: 60px;
    height: 60px;
  }

  .track-title {
    font-size: 1.2rem;
  }

  .control-buttons {
    gap: 1rem;
  }
}
</style>
