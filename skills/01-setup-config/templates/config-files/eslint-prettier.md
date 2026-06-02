# ESLint + Prettier มาตรฐาน (copy ได้เลย)

## .eslintrc.json

```json
{
  "extends": ["next/core-web-vitals"],
  "rules": {
    "react-hooks/exhaustive-deps": "error",
    "no-unused-vars": ["warn", {
      "argsIgnorePattern": "^_",
      "varsIgnorePattern": "^_",
      "destructuredArrayIgnorePattern": "^_"
    }],
    "prefer-const": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

## ⚠️ Next.js 14 Compatibility

```
❌ "next/typescript" — ไม่มีใน Next.js 14, ใช้แล้ว ESLint fail
✅ ใช้แค่ "next/core-web-vitals" เท่านั้น

เหตุผล: "next/typescript" ถูกเพิ่มใน eslint-config-next เวอร์ชันใหม่กว่า 14
       Next.js 14 ใช้ "next/core-web-vitals" ครอบทุก TypeScript rule ที่จำเป็น
```

## ⚠️ no-unused-vars ต้องมี 3 ignore patterns

```
argsIgnorePattern          → function argument ที่ขึ้นต้น _
varsIgnorePattern           → variable ใน type declaration ที่ขึ้นต้น _
destructuredArrayIgnorePattern → destructured { confirm: _c } ที่ขึ้นต้น _

ถ้าขาด varsIgnorePattern → type parameter ชื่อ setUser(value) ใน Zustand store
                            จะถูก warn "value is defined but never used"
```

## ⚠️ กฎสำคัญ

`react-hooks/exhaustive-deps` ต้องเป็น `"error"` เสมอ — ไม่ใช่ `"warn"`

```
"warn" → developer มองข้าม → eslint-disable เต็ม codebase ใน 2 เดือน
"error" → บังคับแก้ก่อน commit → ป้องกัน stale closure ตั้งแต่แรก
```

---

## prettier.config.js

```js
/** @type {import('prettier').Config} */
const config = {
  semi: false,
  singleQuote: true,
  tabWidth: 2,
  trailingComma: 'es5',
  printWidth: 100,
  plugins: ['prettier-plugin-tailwindcss'],
}

module.exports = config
```

## .prettierignore

```
.next
node_modules
public
```

## package.json scripts (เพิ่ม)

```json
{
  "scripts": {
    "lint": "next lint",
    "lint:fix": "next lint --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit"
  }
}
```
