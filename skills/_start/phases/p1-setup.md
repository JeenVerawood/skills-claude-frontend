# Phase 1 — Skill 00: Project Setup

**Summary Phase 0 ก่อนเริ่ม** (3-5 bullet: project type, features หลัก, tech decisions จาก spec)

**ไฟล์ที่ต้องอ่าน:**
```
../01-setup-config/references/dependencies.md
../01-setup-config/references/project-checklist.md
../01-setup-config/bad-patterns/setup-mistakes.md
../01-setup-config/templates/config-files/next-config.md
../01-setup-config/templates/config-files/eslint-prettier.md
../01-setup-config/templates/config-files/tailwind-postcss.md
../01-setup-config/templates/config-files/gitignore.md
```

**ขั้นตอน:**
1. แสดง `npx create-next-app@latest` command พร้อม flags ที่ถูกต้อง
2. แสดง install commands สำหรับ dependencies ทั้งหมด (จาก spec)
3. สร้าง `next.config.js` (CommonJS: `module.exports = nextConfig` — ห้ามใช้ next.config.ts หรือ export default)
4. สร้าง `.eslintrc.json` + `prettier.config.js`
5. สร้าง `tailwind.config.ts` + `postcss.config.js`
6. สร้าง `.gitignore`
7. สร้าง `CLAUDE.md` ที่ root (จาก `../01-setup-config/templates/claude-md.md`)

**✅ GATE 1:** ยืนยันว่า config files ครบ 7 ไฟล์ก่อนไป Phase 2

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 1: Setup
**Type**: Phase
**Skill Used**: _start + 01-setup-config
**Action**: สร้าง config files และ dependencies
**Files Created**:
- next.config.js
- .eslintrc.json
- prettier.config.js
- tailwind.config.ts
- postcss.config.js
- .gitignore
- CLAUDE.md
**Gate**: ✅
```
