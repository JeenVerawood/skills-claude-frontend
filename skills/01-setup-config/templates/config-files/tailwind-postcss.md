# Tailwind + PostCSS มาตรฐาน (copy ได้เลย)

## tailwind.config.ts

```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-sans)', 'system-ui', 'sans-serif'],
      },
      colors: {
        // เพิ่ม brand colors ที่นี่
        // primary: { 50: '#...', 500: '#...', 900: '#...' }
      },
      borderRadius: {
        // เพิ่ม custom radius ถ้าต้องการ
      },
    },
  },
  plugins: [
    // require('@tailwindcss/forms'),
    // require('@tailwindcss/typography'),
  ],
}

export default config
```

## postcss.config.js

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

## src/app/globals.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply bg-white text-gray-900 antialiased;
  }
}
```

## ⚠️ NEVER copy shadcn/ui CSS Variables pattern โดยไม่ได้ setup shadcn/ui

```css
/* ❌ pattern นี้จะ error ถ้าไม่ได้ใช้ shadcn/ui */
@layer base {
  :root {
    --background: 0 0% 100%;
    --border: 214.3 31.8% 91.4%;
  }
  * { @apply border-border; }      /* ERROR: class ไม่มีอยู่ */
  body { @apply bg-background; }   /* ERROR: class ไม่มีอยู่ */
}
```

`border-border`, `bg-background`, `text-foreground` ต้องการ colors extend ใน tailwind.config:
```ts
// ต้องมีแบบนี้ใน tailwind.config.ts — shadcn/ui เพิ่มให้อัตโนมัติ
colors: {
  border: 'hsl(var(--border))',
  background: 'hsl(var(--background))',
  foreground: 'hsl(var(--foreground))',
}
```
ถ้าไม่ได้ใช้ shadcn/ui → ใช้ Tailwind built-in classes (`bg-white`, `text-gray-900`) โดยตรงเท่านั้น
