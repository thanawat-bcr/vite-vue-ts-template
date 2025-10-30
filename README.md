# Create Vite + Vue + TypeScript

## Init Project
1. `npm create vite@latest --project_name`
2. Select a framework: `Vue`
3. Select a variant: `Official Vue Starter`
4. Use rolldown-vite: `No`
5. Install with npm and start now?: `Yes`
6. Select features: ( Your Desired )
- [x] TypeScript
- [ ] JSX Support
- [x] Router (SPA development)
- [x] Pinia (state management)
- [ ] Vitest (unit testing)
- [ ] End-to-End Testing
- [x] ESLint (error prevention)
- [ ] Prettier (code formatting)
7. Select experimental features: `None`
8. Skip all example code: `No`
9. `cd --project_name && npm install`

## Install Dependencies ( Optional )
### Eslint [@antfu/eslint-config](https://github.com/antfu/eslint-config)

1. Installation
```sh
npm install -D @antfu/eslint-config eslint-plugin-format
```
2. Update `eslint.config.ts`
```ts
import antfu from '@antfu/eslint-config'

export default antfu({
  formatters: true,
  vue: {
    overrides: {
      'vue/block-order': ['error', {
        order: ['template', 'script', 'style'],
      }],
    },
  },
  rules: {
    'no-console': 'off',
  },
})

```
3. Update `package.json` ( Optional )
```diff
  "scripts": {
    "dev": "vite",
    ...
+   "lint": "eslint",
+   "lint:fix": "eslint --fix"
  },
```
4. Test
```sh
npm run lint
npm run lint:fix
```
#### If your IDE cannot format on save
Rename from `eslint.config.ts` to `esling.config.js`

### Tailwind CSS V4 + SASS
1. Installation
```sh
npm install tailwindcss @tailwindcss/vite
npm install -D sass
```
2. Update `vite.config.ts`
```diff
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    // ...
+   tailwindcss(),
  ],
})
```
3. Create `src/assets/styles.css`
```css
@import "tailwindcss";
```
4. Update `main.ts`
```diff
// ...
+ import '@/assets/styles.css'
// ...
```
### PrimeVue V4 with Tailwind CSS
1. Installation
```sh
npm install primevue @primeuix/themes tailwindcss-primeui
```
2. Update `main.ts`
```diff
+ import { definePreset } from '@primeuix/themes'
+ import Aura from '@primeuix/themes/aura'
+ import PrimeVue from 'primevue/config'

// ...
// OPTIONAL PRIMEVUE PRESET
const PrimeVueAuraCustomPreset = definePreset(Aura, {
  semantic: {
    primary: {
      50: '{emerald.50}',
      100: '{emerald.100}',
      200: '{emerald.200}',
      300: '{emerald.300}',
      400: '{emerald.400}',
      500: '{emerald.500}',
      600: '{emerald.600}',
      700: '{emerald.700}',
      800: '{emerald.800}',
      900: '{emerald.900}',
      950: '{emerald.950}',
    },
  },
})
// OR USE DEFAULT AURA OR ANY THEME YOU DESIRED
app.use(PrimeVue, {
  theme: {
    preset: PrimeVueAuraCustomPreset,
    options: {
      darkModeSelector: 'none',
      cssLayer: {
        name: 'primevue',
        order: 'base, theme, primevue',
      },
    },
  },
})
```
3. Update `styles.css`
```diff
@import "tailwindcss";
+ @import "tailwindcss-primeui";
```

### Auto Import and Components
1. Installation
```sh
npm install -D unplugin-auto-import unplugin-vue-components @primevue/auto-import-resolver   
```
2. Update `vite.config.ts`
```diff
+ import { PrimeVueResolver } from '@primevue/auto-import-resolver'
+ import AutoImport from 'unplugin-auto-import/vite'
+ import Components from 'unplugin-vue-components/vite'
// ...
  plugins: [
    vue(),
    vueDevTools(),
    tailwindcss(),
+   AutoImport({
+     imports: ['vue', 'vue-router', 'pinia'],
+   }),
+   Components({
+     globs: [],
+     resolvers: [PrimeVueResolver()],
+   }),
  ],
```
3. Update `.gitignore`
```diff
+ auto-imports.d.ts
+ components.d.ts
```
