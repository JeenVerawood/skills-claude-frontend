# LOG.md — Project Activity Log

> บันทึกทุก action ที่เกิดขึ้นใน project นี้แบบ real-time
> อัปเดตทันทีหลังทุก action — ห้าม batch ทีหลัง

---

## Project Info

| Field | Value |
|-------|-------|
| **Project Name** | `[ชื่อ project]` |
| **Started** | `[YYYY-MM-DD]` |
| **Stack** | Next.js 15 / TypeScript / Tailwind |

---

## Entry Format

### Phase Entry (จาก _start)
```
## [NNN] YYYY-MM-DD | _start — Phase X: [ชื่อ Phase]
**Type**: Phase
**Skill Used**: _start + [sub-skill เช่น 01-setup-config]
**Action**: [อธิบายสั้นๆ ว่าทำอะไร]
**Files Created**:
- path/to/file.ts
- path/to/file.tsx
**Files Modified**:
- path/to/file.ts — [อธิบายว่าเปลี่ยนอะไร]
**Decisions**:
- [decision] — เหตุผล: [reason]
**Gate**: ✅ ผ่าน / ❌ ยังไม่ผ่าน
```

### Bug Fix Entry (จากทุก skill)
```
## [NNN] YYYY-MM-DD | [skill-name] — Bug Fix: [ชื่อ bug สั้นๆ]
**Type**: Bug Fix
**Trigger**: [error message หรือ user report]
**Root Cause**: [ทำไมถึงเกิด]
**Files Modified**:
- path/to/file — [อธิบายว่าเปลี่ยนอะไร]
**Fix**: [สิ่งที่ทำเพื่อแก้]
**Skill Updated**: yes / no — [ถ้า yes ระบุไฟล์ skill ที่แก้]
```

### Feature/Change Entry (จากทุก skill)
```
## [NNN] YYYY-MM-DD | [skill-name] — [ชื่อ feature/change]
**Type**: Feature / Refactor / Config / Fix
**Trigger**: [user request หรือ requirement]
**Files Created**:
- path/to/file.ts
**Files Modified**:
- path/to/file.ts — [อธิบาย]
**Notes**: [ข้อมูลเพิ่มเติม decision หรือ tradeoff]
```

### Skill Update Entry
```
## [NNN] YYYY-MM-DD | skill-update — [skill-name]: [หัวข้อ]
**Type**: Skill Update
**Trigger**: [bug หรือ pattern ที่ทำให้ต้องอัปเดต skill]
**Skill Files Modified**:
- .claude/skills/[skill]/[file] — [เพิ่ม/แก้อะไร]
**Lesson**: [pattern ที่เรียนรู้ เพื่อป้องกันครั้งหน้า]
```

---

## Activity Log

<!-- entries เพิ่มด้านล่างนี้ เรียงจากเก่าสุด → ใหม่สุด (append ต่อท้าย) -->

## [001] [YYYY-MM-DD] | _start — Phase 0: REQ Analysis
**Type**: Phase
**Skill Used**: _start + 00-req-spec
**Action**: วิเคราะห์ requirement สร้าง Frontend Spec
**Decisions**:
- 
**Gate**: ✅

---
