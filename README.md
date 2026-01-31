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
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite --host 0.0.0.0 --port 3000",
    "dev:mobile": "vite --host 0.0.0.0 --port 3000 --mode development",
    "build": "tsc && vite build",
    "preview": "vite preview --port 4173",
    "deploy": "vercel --prod",
    "test": "vitest",
    "test:watch": "vitest --watch",
    "lint": "eslint . --ext .vue,.js,.jsx,.cjs,.mjs,.ts,.tsx,.cts,.mts --fix --ignore-path .gitignore",
    "format": "prettier --write src/"
  },
  "dependencies": {
    "@deepseek/deepseek": "^1.1.0",
    "lucide-vue-next": "^0.358.0",
    "pinia": "^2.1.7",
    "vue": "^3.4.0",
    "vue-router": "^4.2.5",
    "vant": "^4.9.0"
  },
  "devDependencies": {
    "@types/node": "^20.11.0",
    "@typescript-eslint/eslint-plugin": "^7.0.0",
    "@typescript-eslint/parser": "^7.0.0",
    "@vitejs/plugin-vue": "^5.0.0",
    "@vue/test-utils": "^2.4.5",
    "autoprefixer": "^10.4.17",
    "eslint": "^8.56.0",
    "eslint-plugin-vue": "^9.20.1",
    "jsdom": "^23.2.0",
    "postcss": "^8.4.33",
    "prettier": "^3.2.5",
    "sass": "^1.69.5",
    "typescript": "~5.3.0",
    "vite": "^5.0.0",
    "vite-plugin-pwa": "^0.17.4",
    "vitest": "^1.2.0",
    "vue-tsc": "^1.8.27"
  },
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=9.0.0"
  },
  "keywords": [
    "ai",
    "chatbot",
    "mobile",
    "vue",
    "vercel"
  ],
  "author": "Your Name",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/youme-ai.git"
  },
  "bugs": {
    "url": "https://github.com/yourusername/youme-ai/issues"
  },
  "homepage": "https://youme-ai.vercel.app"
}
