# Phase 6 — Skill 05 + 06: API + State Management

**Summary Phase 5 ก่อนเริ่ม** (rendering strategy ต่อ route)

## ขอบเขต (Frontend เท่านั้น)

```
✅ ทำ:
- Route Handlers ที่ proxy ไป external backend API (ไม่ใช่ call DB โดยตรง)
- Server Actions ที่เรียก fetch() ไปยัง external backend API
- TanStack Query hooks สำหรับ data fetching
- Zustand store สำหรับ global UI state
- API client helper ที่ fetch() ไป external backend

❌ ห้ามทำ:
- ไม่สร้างไฟล์ที่ import db, prisma, mongoose หรือ ORM ใดๆ
- ไม่สร้าง database connection หรือ schema
- ถ้า requirement มี backend logic → ระบุว่าเป็น "API ที่ frontend ต้อง call" แล้วทำแค่ frontend side
```

**ไฟล์ที่ต้องอ่าน:**
```
../06-api-routes/references/route-handler-pattern.md
../06-api-routes/references/server-actions.md
../06-api-routes/references/fetch-patterns.md
../06-api-routes/bad-patterns/api-mistakes.md
../07-state-data/references/decision-matrix.md
../07-state-data/references/zustand-patterns.md
../07-state-data/references/tanstack-query.md
../07-state-data/bad-patterns/state-mistakes.md
../05-ts-zod/references/zod-patterns.md
```

**ขั้นตอน:**
1. แสดง Decision Matrix: Server Action vs Route Handler ต่อ use case
2. สร้าง API client helper (`src/lib/api/client.ts`) สำหรับ fetch ไป external backend
3. สร้าง Route Handlers ตาม spec — เป็น **proxy** ไปยัง external API พร้อม Zod validation + auth guard + error handling
4. สร้าง Server Actions ถ้า spec ต้องการ form mutation — เรียก fetch() ไป external API
5. แสดง Decision Matrix: state tool ที่เหมาะสม (TanStack Query / Zustand / useState / URL)
6. ตั้งค่า TanStack Query provider + custom hooks ตาม spec
7. สร้าง Zustand store ถ้า spec ต้องการ global UI state

**✅ GATE 6:** ยืนยัน
- ทุก Route Handler ทำหน้าที่ proxy — ไม่มี db.* หรือ ORM ใดๆ ในโค้ด
- ทุก API route มี: auth check + Zod validation + error handling + HTTP status ถูกต้อง
- State management tool เลือกตาม decision matrix (ไม่ over-engineer)

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 6: API + State Management
**Type**: Phase
**Skill Used**: _start + 06-api-routes + 07-state-data
**Action**: สร้าง API proxy routes, Zustand store, TanStack Query hooks
**Files Created**:
- src/lib/api/client.ts
- src/app/api/[route]/route.ts (list ทุก route)
- src/stores/[name]Store.ts (list)
**Notes**: ทุก route เป็น proxy — ไม่มี DB call
**Gate**: ✅
```
