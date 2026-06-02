# Phase 2 — Skill 01 + 04: Structure + TypeScript Foundation

**Summary Phase 1 ก่อนเริ่ม** (config files ที่สร้าง, dependencies ที่ install)

**ไฟล์ที่ต้องอ่าน:**
```
../02-folder-types/references/folder-structure.md
../02-folder-types/references/component-rules.md
../02-folder-types/references/tsconfig-paths.md
../02-folder-types/bad-patterns/common-mistakes.md
../02-folder-types/templates/layer-folder.md
../02-folder-types/templates/feature-folder.md
../05-ts-zod/references/tsconfig-standard.md
../05-ts-zod/references/type-conventions.md
../01-setup-config/templates/boilerplate/types.md
```

**ขั้นตอน:**
1. เลือก folder pattern ตาม project size จาก spec
   - Layer: โปรเจกต์กลาง หรือ cross-feature collaboration บ่อย
   - Feature: โปรเจกต์ใหญ่ ทีมแยกทำ feature ชัดเจน
2. รัน scaffold commands จาก template ที่เลือก — `mkdir -p` + `touch` ทุกไฟล์ใน spec
3. แสดง folder tree ที่สมบูรณ์พร้อม path จริง
4. สร้าง `tsconfig.json` ตาม strict mode standard
5. สร้าง `src/types/` ทั้งหมดตาม spec

**✅ GATE 2:** ไฟล์และ folder ถูกสร้างจริง + tsconfig + base types พร้อมก่อนไป Phase 3

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 2: Structure + TypeScript
**Type**: Phase
**Skill Used**: _start + 02-folder-types + 05-ts-zod
**Action**: สร้าง folder structure, tsconfig, base types
**Decisions**:
- Folder pattern: layer / feature — เหตุผล: [reason]
**Files Created**:
- tsconfig.json
- src/types/index.ts, api.types.ts, common.types.ts, [domain].types.ts
- src/constants/index.ts
**Gate**: ✅
```
