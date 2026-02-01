#!/bin/bash

echo "🚀 YouMe 一键部署脚本"
echo "======================"

# 检查是否已安装Vercel CLI
if ! command -v vercel &> /dev/null; then
    echo "📦 安装 Vercel CLI..."
    npm install -g vercel
fi

# 创建项目结构
echo "📁 创建项目结构..."
mkdir -p youme-web
cd youme-web

# 创建必要目录
mkdir -p public src/components src/pages src/utils src/api src/styles
mkdir -p src/components/Chat src/components/Diary src/components/Moments 
mkdir -p src/components/GroupChat src/components/ListenTogether src/components/UI
mkdir -p api

# 创建 package.json
cat > package.json << 'EOF'
{
  "name": "youme-web",
  "version": "1.0.0",
  "description": "YouMe - 专属AI陪伴社交网页版",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "deploy": "vercel --prod"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.8.1",
    "axios": "^1.3.4",
    "localforage": "^1.10.0",
    "uuid": "^9.0.0",
    "dayjs": "^1.11.7",
    "weui": "^2.4.4",
    "emoji-mart": "^5.2.5",
    "react-image-crop": "^10.0.6",
    "react-dropzone": "^14.2.3",
    "react-lazyload": "^3.2.0",
    "react-textarea-autosize": "^8.3.4",
    "react-virtualized": "^9.22.3"
  },
  "devDependencies": {
    "@types/react": "^18.0.28",
    "@types/react-dom": "^18.0.11",
    "@vitejs/plugin-react": "^3.1.0",
    "autoprefixer": "^10.4.13",
    "postcss": "^8.4.21",
    "tailwindcss": "^3.2.7",
    "vite": "^4.1.4",
    "vercel": "^28.16.0"
  },
  "engines": {
    "node": ">=16.0.0"
  }
}
EOF

# 创建 vercel.json
cat > vercel.json << 'EOF'
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite",
  "outputDirectory": "dist",
  "github": {
    "silent": false
  },
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        }
      ]
    }
  ]
}
EOF

# 创建 index.html
cat > public/index.html << 'EOF'
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="theme-color" content="#07c160">
  <meta name="description" content="YouMe - 专属AI陪伴社交，无敏感词自由表达，永久记忆">
  <title>YouMe - 专属AI陪伴</title>
  <link rel="icon" href="/favicon.ico" type="image/x-icon">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/weui@2.4.4/dist/style/weui.min.css">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: -apple-system, BlinkMacSystemFont, sans-serif; }
    #app { position: fixed; top: 0; left: 0; right: 0; bottom: 0; }
  </style>
</head>
<body>
  <div id="app"></div>
  <script type="module" src="/src/index.js"></script>
</body>
</html>
EOF

# 创建 API 代理
cat > api/siliconflow.js << 'EOF'
export default async function handler(req, res) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET,POST,OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  if (req.method === 'OPTIONS') {
    res.status(200).end();
    return;
  }

  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  try {
    const { prompt, aiConfig } = req.body;
    const API_KEY = process.env.SILICONFLOW_API_KEY;

    if (!API_KEY) {
      return res.status(400).json({ error: 'API key not configured' });
    }

    const response = await fetch('https://api.siliconflow.cn/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${API_KEY}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        model: "Qwen/Qwen2.5-7B-Instruct",
        messages: [
          {
            role: "system",
            content: `你是${aiConfig.name}，人设：${aiConfig.personality}`
          },
          {
            role: "user",
            content: prompt
          }
        ],
        max_tokens: 500,
        temperature: 0.7
      })
    });

    const data = await response.json();
    res.status(200).json(data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
}
EOF

# 创建 README.md
cat > README.md << 'EOF'
# YouMe - 专属AI陪伴社交网页版

## ✨ 功能特性

### 核心功能
- 💬 **一对一AI聊天** - 无敏感词限制，永久记忆
- 📱 **微信风格界面** - 熟悉的交互逻辑
- 📝 **5000字AI人设自定义** - 打造专属AI
- 📷 **图片/表情包聊天** - 丰富的互动形式
- 🎵 **一起听歌** - 同步播放+文字聊天
- 🎮 **双人小游戏** - 多种休闲游戏
- 📔 **日记本** - 用户和AI都能写日记
- 📱 **朋友圈** - 用户和AI都能发动态
- 👥 **群聊** - 和多个AI一起聊天
- 🎨 **全功能自定义** - 头像、昵称、背景自由设置

### 技术特性
- ⚡ **轻量化加载** - 首次加载<3秒
- 📱 **响应式设计** - 完美适配手机
- 🔒 **数据安全** - 本地存储+端到端加密
- 🔄 **实时更新** - 无刷新流畅体验
- 🌙 **深色模式** - 护眼夜间模式

