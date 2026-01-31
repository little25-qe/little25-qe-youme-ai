# little25-qe-youme-ai
Vue项目，用于Vercel部署
youme-ai/
├── package.json
├── vite.config.ts
├── index.html
├── tsconfig.json
├── tsconfig.node.json
├── vercel.json
├── public/
│   ├── favicon.ico
│   ├── icon-192.png
│   └── icon-512.png
└── src/
    ├── main.ts
    ├── App.vue
    ├── env.d.ts
    ├── styles/
    │   └── main.scss
    ├── components/
    │   ├── MobileOnlyFeatures.vue
    │   ├── ChatBubble.vue
    │   ├── ChatInput.vue
    │   ├── AIMenu.vue
    │   └── LoadingIndicator.vue
    ├── views/
    │   ├── Home.vue
    │   └── Settings.vue
    ├── router/
    │   └── index.ts
    ├── stores/
    │   └── chat.ts
    ├── utils/
    │   ├── gestures.ts
    │   └── mobile.ts
    └── types/
        └── index.ts
{
  "name": "youme-ai",
  "version": "1.0.0",
  "description": "移动端AI伴聊应用",
  "type": "module",
  "scripts": {
    "dev": "vite --host 0.0.0.0",
    "build": "vue-tsc --noEmit && vite build",
    "preview": "vite preview --host 0.0.0.0",
    "deploy": "npm run build && vercel --prod"
  },
  "dependencies": {
    "vue": "^3.4.0",
    "vue-router": "^4.2.0",
    "pinia": "^2.1.0",
    "axios": "^1.6.0",
    "@vercel/analytics": "^1.0.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.5.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "vue-tsc": "^1.8.0",
    "@types/node": "^20.0.0",
    "sass": "^1.69.0",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  
  resolve: {
    alias: {
      '@': resolve(__dirname, './src'),
    },
  },
  
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `
          @import "@/styles/_variables.scss";
          @import "@/styles/_mixins.scss";
        `,
      },
    },
  },
  
  server: {
    host: '0.0.0.0',
    port: 5173,
    strictPort: true,
    hmr: {
      clientPort: 5173,
    },
  },
  
  build: {
    target: 'es2020',
    outDir: 'dist',
    assetsDir: 'assets',
    sourcemap: false,
    minify: 'esbuild',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['vue', 'vue-router', 'pinia', 'axios'],
        },
      },
    },
    chunkSizeWarningLimit: 1000,
  },
  
  optimizeDeps: {
    include: ['vue', 'vue-router', 'pinia'],
  },
})
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite",
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ],
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/(.*\\.(js|css))",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=604800"
        }
      ]
    }
  ],
  "env": {
    "NODE_ENV": "production"
  }
}
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,

    /* Paths */
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },

    /* Vue Support */
    "types": ["vite/client"],
    "allowSyntheticDefaultImports": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue"
  ],
  "references": [{ "path": "./tsconfig.node.json" }]
}
<!doctype html>
<html lang="zh-CN">
 head>
   meta charset="UTF-8" />
   link rel="icon" type="image/svg+xml" href="/favicon.ico" />
   meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
    
    <!-- iOS PWA 配置 -->
   meta name="apple-mobile-web-app-capable" content="yes">
   meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
   meta name="apple-mobile-web-app-title" content="YouMe AI">
   meta name="format-detection" content="telephone=no">
   meta name="theme-color" content="#007AFF">
    
    <!-- PWA Manifest -->
   link rel="manifest" href="/manifest.json">
    
    <!-- iOS 图标 -->
   link rel="apple-touch-icon" href="/icon-192.png">
   link rel="apple-touch-startup-image" href="/apple-splash.png">
    
    <!-- Android 主题颜色 -->
   meta name="theme-color" content="#007AFF">
   meta name="description" content="你的专属AI虚拟好友 - 随时随地陪伴你聊天、学习、娱乐">
    
    <!-- Vercel Analytics -->
   script src="https://unpkg.com/@vercel/analytics" deferscript>
    
   title>YouMe AI - 你的AI伴侣title>
 head>
 body>
   div id="appdiv>
   script type="module" src="/src/main.tsscript>
    
    <!-- 移动端检测脚本 -->
   script>
      // 检测设备类型
      (function() {
        const ua = navigator.userAgent;
        const isMobile = /Android|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(ua);
        const isIOS = /iPhone|iPad|iPod/i.test(ua);
        const isAndroid = /Android/i.test(ua);
        const isPWA = window.matchMedia('(display-mode: standalone)').matches;
        
        window.deviceInfo = {
          isMobile,
          isIOS,
          isAndroid,
          isPWA,
          userAgent: ua,
          platform: navigator.platform
        };
        
        // 添加到html标签方便CSS使用
        document.documentElement.classList.add(
          isMobile ? 'mobile' : 'desktop',
          isIOS ? 'ios' : '',
          isAndroid ? 'android' : ''
        );
        
        if (isPWA) {
          document.documentElement.classList.add('pwa');
        }
      })();
   script>
 body>
html>
{
  "name": "YouMe AI伴侣",
  "short_name": "YouMe",
  "description": "你的专属AI虚拟好友，随时随地陪伴你",
  "start_url": "/",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#FFFFFF",
  "theme_color": "#007AFF",
  "categories": ["social", "lifestyle", "productivity"],
  
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icon-512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ],
  
  "shortcuts": [
    {
      "name": "开始新的对话",
      "short_name": "新聊天",
      "description": "和AI开始一段新的对话",
      "url": "/?new=true",
      "icons": [{ "src": "/icon-chat.png", "sizes": "96x96" }]
    },
    {
      "name": "选择AI性格",
      "short_name": "选择AI",
      "description": "选择不同的AI性格聊天",
      "url": "/?change-ai=true",
      "icons": [{ "src": "/icon-ai.png", "sizes": "96x96" }]
    }
  ],
  
  "screenshots": [
    {
      "src": "/screenshot-mobile.png",
      "sizes": "1080x1920",
      "type": "image/png",
      "form_factor": "narrow",
      "label": "手机聊天界面"
    }
  ],
  
  "related_applications": [],
  "prefer_related_applications": false
}
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'
import './styles/main.scss'

// Vercel Analytics
import { inject } from '@vercel/analytics'

const app = createApp(App)

// 设备信息
const isMobile = {
  android: () => /android/i.test(navigator.userAgent),
  ios: () => /iphone|ipad|ipod/i.test(navigator.userAgent),
  any: () => /android|iphone|ipad|ipod/i.test(navigator.userAgent),
  standalone: () => window.matchMedia('(display-mode: standalone)').matches
}

// 全局提供设备信息
app.provide('device', {
  isMobile: isMobile.any(),
  isIOS: isMobile.ios(),
  isAndroid: isMobile.android(),
  isPWA: isMobile.standalone(),
  platform: isMobile.ios() ? 'ios' : isMobile.android() ? 'android' : 'web'
})

// 全局错误处理
app.config.errorHandler = (err, instance, info) => {
  console.error('Vue Error:', err, 'Info:', info, 'Instance:', instance)
  // 可以发送到错误监控服务
}

// 使用插件
app.use(createPinia())
app.use(router)

// 注入Vercel Analytics
inject()

app.mount('#app')

// 添加离线检测
if ('serviceWorker' in navigator && import.meta.env.PROD) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js').catch(error => {
      console.error('Service Worker registration failed:', error)
    })
  })
}
template>
 div class="app" :class="{ 'mobile': device.isMobile, 'pwa': device.isPWA }">
    <!-- 移动端状态栏 -->
   div v-if="device.isMobile && !device.isPWA" class="status-bar">
     div class="status-bar-time">{{ currentTimediv>
     div class="status-bar-icons">
       span class="signal">📶span>
       span class="wifi">📡span>
       span class="battery">🔋 100%span>
     div>
   div>
    
    <!-- 主路由 -->
   router-view v-slot="{ Component }">
     transition name="fade" mode="out-in">
       component :is="Component" />
     transition>
   router-view>
    
    <!-- 移动端底部导航 -->
   nav v-if="device.isMobile" class="mobile-nav">
     router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }">
       span class="icon">💬span>
       span class="label">聊天span>
     router-link>
      
     router-link to="/ais" class="nav-item" :class="{ active: $route.path === '/ais' }">
       span class="icon">🤖span>
       span class="label">AI列表span>
     router-link>
      
     div class="center-button" @click="startNewChat">
       span class="plus-icon">+span>
     div>
      
     router-link to="/history" class="nav-item" :class="{ active: $route.path === '/history' }">
       span class="icon">📖span>
       span class="label">历史span>
     router-link>
      
     router-link to="/settings" class="nav-item" :class="{ active: $route.path === '/settings' }">
       span class="icon">⚙️span>
       span class="label">设置span>
     router-link>
   nav>
    
    <!-- 网络状态提示 -->
   div v-if="!isOnline" class="offline-alert">
     span>⚠️ 网络已断开，正在尝试重新连接span>
   div>
    
    <!-- 加载遮罩 -->
   div v-if="globalLoading" class="loading-overlay">
     div class="loading-spinnerdiv>
     div class="loading-text">加载中div>
   div>
 div>
template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, inject } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const device = inject('device') as any

// 状态
const currentTime = ref('')
const isOnline = ref(navigator.onLine)
const globalLoading = ref(false)

// 更新时间
const updateTime = () => {
  const now = new Date()
  currentTime.value = now.toLocaleTimeString('zh-CN', {
    hour: '2-digit',
    minute: '2-digit'
  })
}

// 网络状态监听
const handleOnline = () => {
  isOnline.value = true
  console.log('网络已恢复')
}

const handleOffline = () => {
  isOnline.value = false
  console.log('网络已断开')
}

// 开启新聊天
const startNewChat = () => {
  globalLoading.value = true
  router.push('/chat/new').finally(() => {
    setTimeout(() => {
      globalLoading.value = false
    }, 500)
  })
}

// 生命周期
onMounted(() => {
  // 时间更新
  updateTime()
  const timeInterval = setInterval(updateTime, 60000)
  
  // 网络状态
  window.addEventListener('online', handleOnline)
  window.addEventListener('offline', handleOffline)
  
  // 清理
  onUnmounted(() => {
    clearInterval(timeInterval)
    window.removeEventListener('online', handleOnline)
    window.removeEventListener('offline', handleOffline)
  })
  
  // 禁用双击缩放（移动端）
  let lastTouchEnd = 0
  document.addEventListener('touchend', (event) => {
    const now = Date.now()
    if (now - lastTouchEnd <= 300) {
      event.preventDefault()
    }
    lastTouchEnd = now
  }, false)
})
script>

<style scoped lang="scss">
.app {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  overflow: hidden;
  
  &.mobile {
    background: #f5f5f7; // iOS风格背景
  }
  
  &.pwa {
    // PWA模式下隐藏状态栏
    .status-bar {
      display: none;
    }
  }
}

.status-bar {
  height: 44px;
  padding: 0 16px;
  background: rgba(0, 0, 0, 0.85);
  color: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  font-weight: 600;
  
  .status-bar-icons {
    display: flex;
    gap: 8px;
    opacity: 0.9;
  }
}

.mobile-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: calc(64px + env(safe-area-inset-bottom));
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding-bottom: env(safe-area-inset-bottom);
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  z-index: 1000;
  
  .nav-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 8px 12px;
    border-radius: 12px;
    text-decoration: none;
    color: #666;
    transition: all 0.2s ease;
    min-width: 56px;
    
    .icon {
      font-size: 22px;
      margin-bottom: 2px;
    }
    
    .label {
      font-size: 10px;
      font-weight: 500;
    }
    
    &.active {
      color: #007AFF;
      background: rgba(0, 122, 255, 0.1);
      
      .icon {
        transform: scale(1.1);
      }
    }
    
    &:active {
      transform: scale(0.95);
    }
  }
  
  .center-button {
    width: 56px;
    height: 56px;
    border-radius: 28px;
    background: linear-gradient(45deg, #007AFF, #5856D6);
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 28px;
    font-weight: 300;
    box-shadow: 
      0 4px 12px rgba(0, 122, 255, 0.3),
      0 10px 30px rgba(0, 122, 255, 0.15);
    transform: translateY(-12px);
    cursor: pointer;
    transition: all 0.2s ease;
    
    &:active {
      transform: translateY(-12px) scale(0.95);
      box-shadow: 
        0 2px 6px rgba(0, 122, 255, 0.3),
        0 5px 15px rgba(0, 122, 255, 0.15);
    }
  }
}

.offline-alert {
  position: fixed;
  top: 44px;
  left: 0;
  right: 0;
  background: #ff3b30;
  color: white;
  padding: 8px 16px;
  font-size: 14px;
  text-align: center;
  z-index: 999;
  animation: slideDown 0.3s ease;
}

.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  
  .loading-spinner {
    width: 48px;
    height: 48px;
    border: 3px solid rgba(255, 255, 255, 0.3);
    border-top-color: white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  
  .loading-text {
    margin-top: 16px;
    color: white;
    font-size: 16px;
  }
}

@keyframes slideDown {
  from {
    transform: translateY(-100%);
  }
  to {
    transform: translateY(0);
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

// 移动端安全区域适配
@supports (padding-top: env(safe-area-inset-top)) {
  .mobile-nav {
    padding-bottom: calc(16px + env(safe-area-inset-bottom));
  }
  
  .status-bar {
    padding-top: env(safe-area-inset-top);
    height: calc(44px + env(safe-area-inset-top));
  }
}

// 页面切换动画
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
style>
import { createRouter, createWebHistory } from 'vue-router'
import Home from '@/views/Home.vue'
import ChatRoom from '@/views/ChatRoom.vue'
import AISelector from '@/components/AISelector.vue'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home,
      meta: {
        title: 'YouMe AI - 你的AI伴侣',
        keepAlive: true
      }
    },
    {
      path: '/chat/:aiId?',
      name: 'Chat',
      component: ChatRoom,
      meta: {
        title: '聊天中...',
        requiresAuth: false
      }
    },
    {
      path: '/ais',
      name: 'AIs',
      component: AISelector,
      meta: {
        title: '选择AI性格',
        keepAlive: true
      }
    },
    {
      path: '/history',
      name: 'History',
      component: () => import('@/views/History.vue'),
      meta: {
        title: '聊天历史'
      }
    },
    {
      path: '/settings',
      name: 'Settings',
      component: () => import('@/views/Settings.vue'),
      meta: {
        title: '设置'
      }
    },
    {
      path: '/:pathMatch(.*)*',
      redirect: '/'
    }
  ],
  
  // 滚动行为
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  }
})

// 路由守卫 - 标题设置
router.beforeEach((to, from, next) => {
  // 设置页面标题
  if (to.meta.title) {
    document.title = to.meta.title as string
  }
  
  // 防止页面缩放（移动端）
  if (window.deviceInfo?.isMobile) {
    const viewportMeta = document.querySelector('meta[name="viewport"]')
    if (viewportMeta && to.name !== 'Home') {
      viewportMeta.setAttribute('content', 
        'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no')
    }
  }
  
  next()
})

export default router
template>
 div class="home">
    <!-- 顶部区域 -->
   div class="top-section">
     div class="welcome">
       h1>👋 你好！我是你的AI好友h1>
       p class="subtitle">随时陪伴，随时倾听，随时帮助p>
     div>
      
     div class="user-avatar" @click="showAISelection">
       div class="avatar-circle">
         span class="avatar-icon">🤖span>
       div>
       div class="avatar-info">
         div class="username">{{ selectedAINamediv>
         div class="status online">在线div>
       div>
     div>
   div>
    
    <!-- 快速开始 -->
   div class="quick-start">
     h2>📱 快速开始h2>
     div class="quick-actions">
       button 
          v-for="action in quickActions" 
          :key="action.id"
          class="quick-action"
          @click="handleQuickAction(action)"
        >
         span class="action-icon">{{ action.iconspan>
         span class="action-title">{{ action.titlespan>
         span class="action-desc">{{ action.descriptionspan>
       button>
     div>
   div>
    
    <!-- AI列表 -->
   div class="ai-section">
     div class="section-header">
       h2>🤖 选择AI性格h2>
       button class="see-all" @click="goToAIs">查看全部 >button>
     div>
      
     div class="ai-list">
       div 
          v-for="ai in featuredAIs" 
          :key="ai.id"
          class="ai-card"
          :class="{ active: ai.id === currentAiId }"
          @click="startChat(ai.id)"
        >
         div class="ai-avatar">
           span class="ai-icon">{{ ai.iconspan>
         div>
         div class="ai-info">
           div class="ai-name">{{ ai.namediv>
           div class="ai-role">{{ ai.rolediv>
           div class="ai-tags">
             span v-for="tag in ai.tags" :key="tag" class="ai-tag">{{ tagspan>
           div>
         div>
         div class="ai-action">
           button class="chat-btn">聊天button>
         div>
       div>
     div>
   div>
    
    <!-- 最近对话 -->
   div class="recent-chats">
     div class="section-header">
       h2>💬 最近对话h2>
       button class="clear-all" @click="clearHistory" v-if="recentChats.length > 0">清空button>
     div>
      
     div v-if="recentChats.length > 0" class="chat-list">
       div 
          v-for="chat in recentChats" 
          :key="chat.id"
          class="chat-item"
          @click="continueChat(chat)"
        >
         div class="chat-avatar">
           span :class="chat.aiIcon">{{ chat.aiIconspan>
         div>
         div class="chat-info">
           div class="chat-header">
             span class="chat-name">{{ chat.aiNamespan>
             span class="chat-time">{{ formatTime(chat.time)span>
           div>
           div class="chat-preview">{{ chat.previewdiv>
         div>
         div class="chat-unread" v-if="chat.unread > 0">
           span class="unread-badge">{{ chat.unreadspan>
         div>
       div>
     div>
      
     div v-else class="empty-state">
       div class="empty-icon">💭div>
       div class="empty-text">暂无对话记录div>
       button class="empty-action" @click="startNewChat">开始第一段对话button>
     div>
   div>
    
    <!-- 移动端提示（只在移动端显示） -->
   div v-if="device.isMobile" class="mobile-tips">
     p>📲strong>PWA安装提示strongp>
     p>点击分享按钮 → 添加到主屏幕 → 即可像原生应用一样使用p>
   div>
 div>
template>

<script setup lang="ts">
import { ref, computed, inject } from 'vue'
import { useRouter } from 'vue-router'
import { useChatStore } from '@/stores/chat'

const router = useRouter()
const chatStore = useChatStore()
const device = inject('device') as any

// 数据
const currentAiId = ref('assistant')
const recentChats = ref([
  {
    id: '1',
    aiId: 'assistant',
    aiName: '智能助手',
    aiIcon: '🤖',
    preview: '你好！有什么可以帮助你的吗？',
    time: Date.now() - 10 * 60 * 1000, // 10分钟前
    unread: 0
  },
  {
    id: '2',
    aiId: 'friend',
    aiName: '虚拟好友',
    aiIcon: '👤',
    preview: '今天天气真好，要不要聊聊最近的心情？',
    time: Date.now() - 2 * 60 * 60 * 1000, // 2小时前
    unread: 2
  }
])

// 精选AI列表
const featuredAIs = ref([
  {
    id: 'assistant',
    name: '智能助手',
    role: '专业的问题解决者',
    icon: '🤖',
    tags: ['专业', '高效', '知识广泛'],
    description: '帮你解决各种问题，提供专业建议'
  },
  {
    id: 'friend',
    name: '虚拟好友',
    role: '倾听陪伴的伙伴',
    icon: '👤',
    tags: ['倾听', '陪伴', '温暖'],
    description: '随时倾听你的心声，分享喜怒哀乐'
  },
  {
    id: 'teacher',
    name: 'AI老师',
    role: '耐心的教育者',
    icon: '👨‍🏫',
    tags: ['教学', '指导', '耐心'],
    description: '帮你学习新知识，解答各种问题'
  },
  {
    id: 'entertainer',
    name: '娱乐达人',
    role: '有趣的伙伴',
    icon: '🎭',
    tags: ['幽默', '有趣', '创意'],
    description: '分享趣事，玩小游戏，陪你放松'
  }
])

// 快速操作
const quickActions = ref([
  {
    id: 'new-chat',
    icon: '💬',
    title: '开始聊天',
    description: '和AI开始新的对话',
    action: () => startNewChat()
  },
  {
    id: 'choose-ai',
    icon: '🤖',
    title: '选择AI',
    description: '挑选不同性格的AI伙伴',
    action: () => goToAIs()
  },
  {
    id: 'share',
    icon: '📲',
    title: '分享应用',
    description: '分享给朋友一起使用',
    action: () => shareApp()
  },
  {
    id: 'feedback',
    icon: '📝',
    title: '反馈建议',
    description: '告诉我们你的想法',
    action: () => router.push('/settings?tab=feedback')
  }
])

// 计算属性
const selectedAIName = computed(() => {
  const ai = featuredAIs.value.find(a => a.id === currentAiId.value)
  return ai?.name || '智能助手'
})

// 方法
const startChat = (aiId: string) => {
  currentAiId.value = aiId
  router.push(`/chat/${aiId}`)
}

const startNewChat = () => {
  router.push('/chat/new')
}

const continueChat = (chat: any) => {
  router.push(`/chat/${chat.aiId}?history=${chat.id}`)
}

const goToAIs = () => {
  router.push('/ais')
}

const showAISelection = () => {
  // 显示AI选择模态框
  console.log('显示AI选择')
}

const clearHistory = () => {
  if (confirm('确定要清空所有聊天记录吗？')) {
    recentChats.value = []
    // TODO: 调用store清空历史
  }
}

const shareApp = () => {
  if (navigator.share) {
    navigator.share({
      title: 'YouMe AI - 你的AI伴侣',
      text: '试试这个超棒的AI聊天应用！',
      url: window.location.href
    })
  } else {
    // 复制链接
    navigator.clipboard.writeText(window.location.href)
    alert('链接已复制到剪贴板！')
  }
}

const handleQuickAction = (action: any) => {
  action.action()
}

const formatTime = (timestamp: number) => {
  const now = Date.now()
  const diff = now - timestamp
  
  if (diff60 * 1000) {
    return '刚刚'
  } else if (diff60 * 60 * 1000) {
    const minutes = Math.floor(diff / (60 * 1000))
    return `${minutes}分钟前`
  } else if (diff24 * 60 * 60 * 1000) {
    const hours = Math.floor(diff / (60 * 60 * 1000))
    return `${hours}小时前`
  } else {
    const days = Math.floor(diff / (24 * 60 * 60 * 1000))
    return `${days}天前`
  }
}
script>

<style scoped lang="scss">
.home {
  flex: 1;
  padding: 16px;
  padding-bottom: calc(64px + env(safe-area-inset-bottom) + 16px);
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  
  .top-section {
    margin-bottom: 24px;
    
    .welcome {
      h1 {
        font-size: 24px;
        font-weight: 700;
        margin-bottom: 8px;
        color: #333;
      }
      
      .subtitle {
        font-size: 14px;
        color: #666;
        margin-bottom: 20px;
      }
    }
    
    .user-avatar {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px;
      background: white;
      border-radius: 16px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
      cursor: pointer;
      transition: transform 0.2s ease;
      
      &:active {
        transform: scale(0.98);
      }
      
      .avatar-circle {
        width: 56px;
        height: 56px;
        border-radius: 28px;
        background: linear-gradient(45deg, #007AFF, #5856D6);
        display: flex;
        align-items: center;
        justify-content: center;
        
        .avatar-icon {
          font-size: 28px;
        }
      }
      
      .avatar-info {
        flex: 1;
        
        .username {
          font-size: 18px;
          font-weight: 600;
          color: #333;
          margin-bottom: 4px;
        }
        
        .status {
          font-size: 12px;
          &.online {
            color: #34C759;
            &::before {
              content: '● ';
              font-size: 8px;
            }
          }
        }
      }
    }
  }
  
  .quick-start {
    margin-bottom: 24px;
    
    h2 {
      font-size: 20px;
      font-weight: 600;
      margin-bottom: 12px;
      color: #333;
    }
    
    .quick-actions {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
      
      .quick-action {
        background: white;
        border-radius: 16px;
        padding: 16px;
        border: none;
        text-align: left;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
        transition: all 0.2s ease;
        cursor: pointer;
        display: flex;
        flex-direction: column;
        
        &:active {
          transform: scale(0.98);
          box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
        }
        
        .action-icon {
          font-size: 24px;
          margin-bottom: 8px;
        }
        
        .action-title {
          font-size: 16px;
          font-weight: 600;
          color: #333;
          margin-bottom: 4px;
        }
        
        .action-desc {
          font-size: 12px;
          color: #666;
          line-height: 1.4;
        }
      }
    }
  }
  
  .ai-section {
    margin-bottom: 24px;
    
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      
      h2 {
        font-size: 20px;
        font-weight: 600;
        color: #333;
      }
      
      .see-all {
        background: none;
        border: none;
        color: #007AFF;
        font-size: 14px;
        cursor: pointer;
        
        &:active {
          opacity: 0.7;
        }
      }
    }
    
    .ai-list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      
      .ai-card {
        background: white;
        border-radius: 16px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
        border: 2px solid transparent;
        transition: all 0.2s ease;
        cursor: pointer;
        
        &.active {
          border-color: #007AFF;
          background: rgba(0, 122, 255, 0.05);
        }
        
        &:active {
          transform: scale(0.995);
          box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
        }
        
        .ai-avatar {
          width: 48px;
          height: 48px;
          border-radius: 24px;
          background: linear-gradient(45deg, #FF9500, #FF5E3A);
          display: flex;
          align-items: center;
          justify-content: center;
          
          .ai-icon {
            font-size: 24px;
          }
        }
        
        .ai-info {
          flex: 1;
          
          .ai-name {
            font-size: 16px;
            font-weight: 600;
            color: #333;
            margin-bottom: 2px;
          }
          
          .ai-role {
            font-size: 12px;
            color: #666;
            margin-bottom: 6px;
          }
          
          .ai-tags {
            display: flex;
            gap: 4px;
            
            .ai-tag {
              font-size: 10px;
              padding: 2px 6px;
              background: #F2F2F7;
              border-radius: 10px;
              color: #666;
            }
          }
        }
        
        .ai-action {
          .chat-btn {
            background: linear-gradient(45deg, #007AFF, #5856D6);
            color: white;
            border: none;
            border-radius: 12px;
            padding: 6px 12px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            
            &:active {
              opacity: 0.8;
            }
          }
        }
      }
    }
  }
  
  .recent-chats {
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      
      h2 {
        font-size: 20px;
        font-weight: 600;
        color: #333;
      }
      
      .clear-all {
        background: none;
        border: none;
        color: #FF3B30;
        font-size: 14px;
        cursor: pointer;
        
        &:active {
          opacity: 0.7;
        }
      }
    }
    
    .chat-list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      
      .chat-item {
        background: white;
        border-radius: 16px;
        padding: 12px;
        display: flex;
        align-items: center;
        gap: 12px;
        box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
        cursor: pointer;
        transition: transform 0.2s ease;
        
        &:active {
          transform: scale(0.995);
        }
        
        .chat-avatar {
          span {
            font-size: 24px;
          }
        }
        
        .chat-info {
          flex: 1;
          min-width: 0; // 防止flex溢出
          
          .chat-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 4px;
            
            .chat-name {
              font-size: 16px;
              font-weight: 600;
              color: #333;
            }
            
            .chat-time {
              font-size: 12px;
              color: #999;
            }
          }
          
          .chat-preview {
            font-size: 14px;
            color: #666;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
          }
        }
        
        .chat-unread {
          .unread-badge {
            width: 20px;
            height: 20px;
            background: #FF3B30;
            color: white;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: 600;
          }
        }
      }
    }
    
    .empty-state {
      background: white;
      border-radius: 16px;
      padding: 40px 20px;
      text-align: center;
      box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
      
      .empty-icon {
        font-size: 48px;
        margin-bottom: 16px;
        opacity: 0.5;
      }
      
      .empty-text {
        font-size: 16px;
        color: #999;
        margin-bottom: 20px;
      }
      
      .empty-action {
        background: linear-gradient(45deg, #007AFF, #5856D6);
        color: white;
        border: none;
        border-radius: 20px;
        padding: 10px 24px;
        font-size: 16px;
        font-weight: 500;
        cursor: pointer;
        
        &:active {
          opacity: 0.8;
        }
      }
    }
  }
  
  .mobile-tips {
    margin-top: 20px;
    padding: 12px;
    background: linear-gradient(45deg, #007AFF, #5856D6);
    border-radius: 16px;
    color: white;
    font-size: 14px;
    line-height: 1.5;
    
    p {
      margin: 4px 0;
      
      strong {
        font-weight: 600;
      }
    }
  }
}

// 响应式调整
@media (min-width: 768px) {
  .home {
    max-width: 500px;
    margin: 0 auto;
    padding-top: 40px;
  }
}

// 安全区域适配
@supports (padding-top: env(safe-area-inset-top)) {
  .home {
    padding-top: calc(env(safe-area-inset-top) + 16px);
  }
}

style>
