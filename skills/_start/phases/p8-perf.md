# Phase 8 — Skill 09: Performance

**Summary Phase 7 ก่อนเริ่ม** (anti-patterns ที่แก้ไป)

**ไฟล์ที่ต้องอ่าน:**
```
../10-perf-bundle/references/bundle-splitting.md
../10-perf-bundle/references/rsc-optimization.md
../10-perf-bundle/references/image-font.md
../10-perf-bundle/bad-patterns/performance-mistakes.md
../10-perf-bundle/templates/dynamic-imports.md
../10-perf-bundle/templates/image-patterns.md
```

**ขั้นตอน:**
1. ตรวจ RSC vs Client Component boundary — ลด `'use client'` ที่ไม่จำเป็น
2. ใส่ `dynamic()` import สำหรับ heavy components
3. ตรวจ `next/image` ใช้ถูกต้อง (width/height, priority, sizes)
4. ตรวจ `next/font` setup ถูกต้อง

**✅ GATE 8:** รัน Final Checklist ก่อนไป Phase 9 (verify-completeness)

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 8: Performance
**Type**: Phase
**Skill Used**: _start + 10-perf-bundle
**Action**: ตรวจและ optimize RSC boundary, dynamic imports, image/font
**Files Modified**:
- [component/page] — [อธิบาย optimization]
**Notes**:
- RSC: ลด 'use client' [X ไฟล์]
- Dynamic imports: [list components]
- next/image: [findings]
**Gate**: ✅
```

```
□ Phase 0  — Frontend Spec สมบูรณ์ ไม่มี 🔴 Blocker ค้าง
□ Phase 1  — Dependencies install + config files ครบ 7 ไฟล์ (รวม CLAUDE.md)
□ Phase 2  — Folder structure + tsconfig + base types พร้อม
□ Phase 3  — env.ts (Zod) + .env.example + utils.ts พร้อม
□ Phase 4  — Auth layer ครบ (หรือ SKIPPED ถ้าไม่มี auth)
□ Phase 5  — Rendering mode กำหนดต่อ route + layout.tsx + health route พร้อม
□ Phase 5b — Page Manifest ✅ ทุก row — ทุก page ใน spec สร้างแล้ว
□ Phase 5c — Component Manifest ✅ ทุก row — ทุก component ใน spec สร้างแล้ว
□ Phase 6  — API routes มี auth+validation+error / State tool เหมาะสม
□ Phase 7  — ไม่มี useEffect anti-pattern
□ Phase 8  — RSC boundary + dynamic imports + next/image ถูกต้อง
```

**🔴 ถ้า Phase 5b หรือ 5c ยังไม่ผ่าน → กลับไปทำก่อน ห้าม proceed**

หลังผ่าน GATE 8 → **ต้องรัน Phase 9 (verify-completeness) ก่อนประกาศ DONE เสมอ**

เมื่อ Phase 9 ผ่าน → แสดง Next Steps:

```
## Project Ready ✓

### Next Steps สำหรับ Developer:
1. Copy .env.example → .env.local แล้วใส่ค่าจริง
2. รัน: pnpm install && pnpm dev
3. ตรวจสอบ http://localhost:3000/api/health
4. [feature-specific steps จาก spec]

### สิ่งที่ต้อง implement ต่อ (นอก scope นี้):
- [Backend API endpoints ที่ frontend รอ]
- [Database migrations]
- [Third-party service credentials]
```
