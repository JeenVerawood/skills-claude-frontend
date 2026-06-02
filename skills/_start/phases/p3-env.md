# Phase 3 — Skill 08: Environment Config

**Summary Phase 2 ก่อนเริ่ม** (folder pattern ที่เลือก, types ที่สร้าง)

**ไฟล์ที่ต้องอ่าน:**
```
../09-env-config/references/env-rules.md
../09-env-config/references/nextconfig.md
../09-env-config/bad-patterns/env-mistakes.md
../09-env-config/templates/env-schema.md
../01-setup-config/templates/boilerplate/env-ts.md
../01-setup-config/templates/boilerplate/utils-ts.md
```

**ขั้นตอน:**
1. สร้าง `src/lib/env.ts` พร้อม Zod validation ครบ env vars ที่ spec ระบุ
2. สร้าง `.env.example` + `.env.local` (ไม่ commit ค่าจริง)
3. สร้าง `src/lib/utils.ts`

**✅ GATE 3:** env validation พร้อม + ไม่มี env var ใดที่ expose ฝั่ง client โดยไม่ตั้งใจ

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 3: Environment Config
**Type**: Phase
**Skill Used**: _start + 09-env-config
**Action**: สร้าง env validation schema และ utils
**Files Created**:
- src/lib/env.ts — Zod schema สำหรับ [list env var names]
- src/lib/utils.ts — cn(), formatDate(), formatCurrency()
- .env.example
**Gate**: ✅
```
