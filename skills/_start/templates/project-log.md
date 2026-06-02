# project.md — Build Process Log

> สร้างโดย skill: start-all-project | อัปเดตหลังทุก Phase Gate ผ่าน

---

## Project Info

| Field | Value |
|-------|-------|
| **Project Name** | `[ชื่อ project]` |
| **Project Type** | `[e-commerce / SaaS / internal tool / content site / other]` |
| **Started** | `[YYYY-MM-DD]` |
| **Spec Location** | `CLAUDE.md → section "Frontend Spec"` |

---

## Spec Summary
_(อัปเดตหลัง Phase 0 ผ่าน Gate)_

- **Pages**: [X pages] — `[list of routes: /, /dashboard, /products, ...]`
- **Auth Required**: yes / no
- **API Endpoints**: `[list: GET /api/products, POST /api/auth/login, ...]`
- **State Tools**: Zustand / TanStack Query / both / none
- **Key Features**: 
  - [feature 1]
  - [feature 2]

---

## Phase Progress

| Phase | Name | Status | Gate |
|-------|------|--------|------|
| 0 | REQ — Requirements | ⏳ | ❌ |
| 1 | Setup | ⏳ | ❌ |
| 2 | Structure + TypeScript | ⏳ | ❌ |
| 3 | Environment Config | ⏳ | ❌ |
| 4 | Auth & Security | ⏳ | ❌ |
| 5 | Next.js Core | ⏳ | ❌ |
| 5b | Pages | ⏳ | ❌ |
| 5c | Components | ⏳ | ❌ |
| 6 | API + State Management | ⏳ | ❌ |
| 7 | React Patterns Audit | ⏳ | ❌ |
| 8 | Performance | ⏳ | ❌ |
| 9 | verify-completeness | ⏳ | ❌ |

_Status legend: ⏳ pending | 🔄 in progress | ✅ done | ⏭️ skipped | ❌ blocked_

---

## Phase Details

### Phase 0 — REQ ⏳

**Status**: ⏳  
**Key Decisions**:
- 

**Blockers Encountered**:
- (none)

---

### Phase 1 — Setup ⏳

**Status**: ⏳  
**Files Created**:
- 

---

### Phase 2 — Structure + TypeScript ⏳

**Status**: ⏳  
**Folder Pattern**: layer / feature  
**Key Decisions**:
- 

**Files/Dirs Created**:
- 

---

### Phase 3 — Environment Config ⏳

**Status**: ⏳  
**Env Vars**:
- 

**Files Created**:
- `src/lib/env.ts`
- `.env.example`

---

### Phase 4 — Auth & Security ⏳

**Status**: ⏳  
**Auth Strategy**: [JWT / session / NextAuth / SKIPPED]  
**Files Created**:
- 

---

### Phase 5 — Next.js Core ⏳

**Status**: ⏳  
**Rendering Modes**:
| Route | Mode |
|-------|------|
| / | static |
| /[route] | dynamic |

**Files Created**:
- 

---

### Phase 5b — Pages ⏳

**Status**: ⏳  
**Pages Created**: [X / X]  

| Route | File | Rendering | Auth |
|-------|------|-----------|------|
| / | src/app/page.tsx | static | no |

---

### Phase 5c — Components ⏳

**Status**: ⏳  
**Total**: [X UI + X Feature = X components]  

**UI Components**: [X ตัว]  
**Feature Components**: [X ตัว]

---

### Phase 6 — API + State ⏳

**Status**: ⏳  
**API Routes Created**:
- 

**State Management**:
- Zustand stores: [list]
- TanStack Query hooks: [list]

---

### Phase 7 — React Patterns Audit ⏳

**Status**: ⏳  
**Anti-patterns Fixed**:
- (none / list of fixes)

---

### Phase 8 — Performance ⏳

**Status**: ⏳  
**Optimizations Applied**:
- RSC boundary: [X `'use client'` removed]
- Dynamic imports: [list of heavy components]
- Image/font: [findings]

---

### Phase 9 — verify-completeness ⏳

**Status**: ⏳  
**Result**: PASS ✅ / FAIL ❌  
**Summary**:
| Category | Expected | Found ✅ | Missing ❌ |
|----------|----------|----------|------------|
| Pages | | | |
| Components | | | |
| API Routes | | | |
| **TOTAL** | | | |

---

## Decisions Log

| Phase | Decision | Reason |
|-------|----------|--------|
| | | |

---

## Blockers Log

| Phase | Blocker | Resolution |
|-------|---------|------------|
| | | |

---

## Resume Instructions
_(อ่านส่วนนี้ถ้า context reset และต้องการ resume งาน)_

1. อ่านไฟล์นี้ (project.md) ครบทั้งหมด
2. ดู Phase Progress Table — หา Phase แรกที่ Gate ยัง ❌
3. อ่าน phase file ของ Phase นั้น (`skills/_start/phases/pN-xxx.md`)
4. อ่าน CLAUDE.md → Frontend Spec เพื่อ refresh context ของ project
5. Resume จาก Phase นั้น — ไม่ต้องเริ่มใหม่จาก Phase 0
