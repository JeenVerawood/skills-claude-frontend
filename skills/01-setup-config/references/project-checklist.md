# Project Setup Checklist

ใช้เป็น checklist ทุกครั้งที่เริ่ม project ใหม่ — ห้ามข้ามขั้นตอน

---

## Phase 1: Install 

```
□ npx create-next-app@latest (--typescript --tailwind --eslint --app --src-dir)
□ npm install zod clsx tailwind-merge @tanstack/react-query zustand react-hook-form @hookform/resolvers
□ npm install -D prettier prettier-plugin-tailwindcss
□ npm run dev → ขึ้น localhost:3000 ได้
```

---

## Phase 2: Config Files

```
□ next.config.js       → security headers + image domains
□ .eslintrc.json       → exhaustive-deps เป็น "error" (ไม่ใช่ warn)
□ prettier.config.js   → prettier-plugin-tailwindcss
□ tailwind.config.ts   → content paths ครบ
□ .gitignore           → ครอบ .env ทุกรูปแบบ
```

---

## Phase 3: Folder Structure 

```
□ app/                          ← route files เท่านั้น
□ src/components/ui/            ← generic components
□ src/components/features/      ← feature-specific components
□ src/hooks/                    ← custom hooks
□ src/lib/                      ← utilities, configs
□ src/types/                    ← TypeScript types
□ src/stores/                   ← Zustand stores
□ src/constants/                ← app-wide constants
```

---

## Phase 4: Boilerplate Files

```
□ src/lib/env.ts                ← Zod env schema
□ src/lib/utils.ts              ← cn(), formatCurrency(), formatDate()
□ src/types/index.ts            ← re-export hub
□ src/types/api.types.ts        ← ApiSuccess, ApiError, PaginatedResponse
□ src/types/common.types.ts     ← Status, BaseEntity, etc.
□ src/app/providers.tsx         ← TanStack Query Provider
□ src/app/layout.tsx            ← font + metadata + Providers
□ src/app/api/health/route.ts   ← health check
□ .env.example                  ← template สำหรับทีม
```

---

## Phase 5: CLAUDE.md 

```
□ CLAUDE.md ที่ root           ← skill routing + chaining rules
```

---

## Phase 6: Verify 

```
□ npm run dev    → ไม่มี error
□ npm run build  → ไม่มี error
□ npx tsc --noEmit → ไม่มี type error
□ npm run lint   → ไม่มี error (ไม่ใช่แค่ warn)
□ git status     → .env ไม่อยู่ใน tracked files
```

---

## Phase 7: Optional (เพิ่มตาม feature)

```
□ API Client      → ดู Skill 05 (API) สำหรับ api/client.ts pattern (fetch ไป external backend)
□ Authentication  → ดู Skill 07
□ Testing         → ดู Skill 10
□ Deployment      → ดู Skill 11
```
