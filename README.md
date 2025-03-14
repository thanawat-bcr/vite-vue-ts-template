# vite-vue-ts-template

This template should help get you started developing with Vue 3 in Vite.

## Setup Guide

### Project Initialize

```sh
npm create vite@latest <name>
```

### Project Setup

1. Setup your project framework and features
- Project name: `Project Name`
- Select a framework: `Vue`
- Select a variant: `Official Vue Starter`
- Select features to include in your project:
  - [x] TypeScript
  - [ ] JSX Support
  - [x] Router (SPA development)
  - [x] Pinia (state management)
  - [ ] Vitest (unit testing)
  - [ ] End-to-End Testing
  - [x] ESLint (error prevention)
  - [ ] Prettier (code formatting)

2. Install dependencies
```sh
cd <name>
npm install
npm run dev
```

### ESLint

We are using `@antfu/eslint-config` for formatting, [Document](https://github.com/antfu/eslint-config)

#### Installation
```sh
npm install -D @antfu/eslint-config eslint-plugin-format
```

#### Update `eslint.config.ts`
```ts
import antfu from '@antfu/eslint-config'

export default antfu()
```
#### Add Script for `package.json`
```diff
  "scripts": {
    "dev": "vite",
    ...
+   "lint": "eslint",
+   "lint:fix": "eslint --fix"
  },
```

#### Update `.vscode/settings.json`
```json
{
  // Disable the default formatter, use eslint instead
  "prettier.enable": false,
  "editor.formatOnSave": false,

  // Auto fix
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "never"
  },

  // Silent the stylistic rules in you IDE, but still auto fix them
  "eslint.rules.customizations": [
    { "rule": "style/*", "severity": "off", "fixable": true },
    { "rule": "format/*", "severity": "off", "fixable": true },
    { "rule": "*-indent", "severity": "off", "fixable": true },
    { "rule": "*-spacing", "severity": "off", "fixable": true },
    { "rule": "*-spaces", "severity": "off", "fixable": true },
    { "rule": "*-order", "severity": "off", "fixable": true },
    { "rule": "*-dangle", "severity": "off", "fixable": true },
    { "rule": "*-newline", "severity": "off", "fixable": true },
    { "rule": "*quotes", "severity": "off", "fixable": true },
    { "rule": "*semi", "severity": "off", "fixable": true }
  ],

  // Enable eslint for all supported languages
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact",
    "vue",
    "html",
    "markdown",
    "json",
    "jsonc",
    "yaml",
    "toml",
    "xml",
    "gql",
    "graphql",
    "astro",
    "svelte",
    "css",
    "less",
    "scss",
    "pcss",
    "postcss"
  ]
}
```

#### Update `.gitignore` (Optional)
If you want your project co-workers to use the same settings, configure and share a settings file.
```diff
# Editor directories and files
.vscode/*
+ !.vscode/extensions.json
+ !.vscode/settings.json
```

#### Fix Initial Format
```sh
npm run lint:fix
```
Every files in your project should be formatted correctly.

### Auto Import and Components

#### Installation
```sh
npm i -D unplugin-auto-import unplugin-vue-components
```

#### Update `vite.config.ts`
```diff
+ import AutoImport from 'unplugin-auto-import/vite'
+ import Components from 'unplugin-vue-components/vite'

export default defineConfig({
  plugins: [
    vue(),
    vueDevTools(),
+   AutoImport({
+     imports: ['vue', 'vue-router', 'pinia'],
+     dirs: ['./src/composables', './src/configs', './src/stores'],
+   }),
+   Components({
+     dirs: ['./src/components', './src/layouts'],
+   }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url)),
    },
  },
})
```
#### Generate `auto-imports.d.ts` and `components.d.ts`
```sh
npm run dev
```
#### Update `.gitignore`
```diff
+ auto-imports.d.ts
+ components.d.ts
```
#### Update `tsconfig.app.json`
```diff
{
  ...
  "include": [
    "env.d.ts",
+   "auto-imports.d.ts",
+   "components.d.ts",
    "src/**/*",
    "src/**/*.vue"
  ],
  ...
}
```

### Primevue + SASS + Tailwind

#### Installation
```sh
npm i primevue @primeuix/themes primeicons
npm i sass tailwindcss @tailwindcss/vite tailwindcss-primeui
npm i -D @primevue/auto-import-resolver
```

#### Create `styles.css`
```css
@import "tailwindcss";
@import "tailwindcss-primeui";
@import 'primeicons/primeicons.css';
```

#### Update `main.ts`
```diff
+ import { definePreset } from '@primeuix/themes'
+ import Aura from '@primeuix/themes/aura'
import { createPinia } from 'pinia'
+ import PrimeVue from 'primevue/config'

import { createApp } from 'vue'
import App from './App.vue'

import router from './router'
- import './assets/main.css'
+ import '@/assets/styles.css'

+ const PrimeVueCustomPreset = definePreset(Aura, {})
const app = createApp(App)

+ app.use(PrimeVue, {
+   theme: {
+     preset: PrimeVueCustomPreset,
+     options: {
+       darkModeSelector: false,
+       cssLayer: {
+         name: 'primevue',
+         order: 'theme, base, primevue',
+       },
+     },
+   },
+ })
app.use(createPinia())
app.use(router)

app.mount('#app')
```

#### Update `vite.config.ts`
```diff
+ import { PrimeVueResolver } from '@primevue/auto-import-resolver'
+ import tailwindcss from '@tailwindcss/vite'
// ...

export default defineConfig({
  plugins: [
    // ...
+   tailwindcss(),
    Components({
      dirs: ['./src/components', './src/layouts'],
+     resolvers: [PrimeVueResolver()],
    }),
    // ...
  ],
})
```
