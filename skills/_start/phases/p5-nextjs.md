# Phase 5 — Skill 02: Next.js Core

**Summary Phase 4 ก่อนเริ่ม** (auth setup หรือ SKIPPED)

**ไฟล์ที่ต้องอ่าน:**
```
../03-nextjs-render/references/rendering-modes.md
../03-nextjs-render/references/caching-layers.md
../03-nextjs-render/references/route-conventions.md
../03-nextjs-render/references/next-image-font.md
../03-nextjs-render/bad-patterns/cache-mistakes.md
../03-nextjs-render/bad-patterns/hydration-mistakes.md
../03-nextjs-render/bad-patterns/route-mistakes.md
../01-setup-config/templates/boilerplate/layout-providers.md
../01-setup-config/templates/boilerplate/health-route.md
```

**ขั้นตอน:**
1. กำหนด rendering strategy ต่อ route จาก spec (static / dynamic / ISR)
2. สร้าง `src/app/layout.tsx` + `src/app/providers.tsx`
3. สร้าง `src/app/api/health/route.ts`
4. ตั้งค่า `loading.tsx` / `error.tsx` / `not-found.tsx` สำหรับ routes หลัก
5. กำหนด caching headers ที่เหมาะสมต่อ route

**✅ GATE 5:** ยืนยัน rendering mode กำหนดครบทุก route ใน spec + ไม่มี hydration mismatch risk

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 5: Next.js Core
**Type**: Phase
**Skill Used**: _start + 03-nextjs-render
**Action**: ตั้งค่า rendering strategy, layout, providers, special files
**Files Created**:
- src/app/layout.tsx — root layout + font
- src/app/providers.tsx — QueryClientProvider
- src/app/globals.css
- src/app/loading.tsx, error.tsx, not-found.tsx
- src/app/api/health/route.ts
**Decisions**:
- [route]: [static/dynamic/ISR] — เหตุผล: [reason]
**Gate**: ✅
```
