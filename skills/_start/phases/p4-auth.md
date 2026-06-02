# Phase 4 — Skill 07: Auth & Security

**⚙️ CONDITIONAL — รัน Phase นี้เมื่อ spec ระบุว่ามี:**
- Login / Logout flow
- Protected routes / pages
- Role-based access (admin, user, ฯลฯ)
- JWT / Session management

**ถ้าไม่มี → ประกาศ `SKIPPED: ไม่มี auth ใน spec` แล้วไป Phase 5**

**Summary Phase 3 ก่อนเริ่ม** (env vars ที่กำหนด)

**ไฟล์ที่ต้องอ่าน:**
```
../08-auth-guard/references/auth-layers.md
../08-auth-guard/references/cookie-security.md
../08-auth-guard/references/rbac.md
../08-auth-guard/bad-patterns/auth-mistakes.md
../08-auth-guard/templates/auth-helpers.md
../08-auth-guard/templates/login-route.md
../03-nextjs-render/templates/middleware-auth.md
```

**ขั้นตอน:**
1. ตั้งค่า auth solution (NextAuth.js / custom JWT) ตาม spec
2. สร้าง `middleware.ts` สำหรับ protected routes
3. สร้าง `src/lib/auth.ts` (getSession, requireAuth, requireRole)
4. สร้าง login/logout API route

**✅ GATE 4:** Security checklist ผ่านก่อนไป Phase 5
- ไม่มี JWT ใน localStorage
- Cookie มี httpOnly + SameSite + Secure
- Protected routes block ที่ middleware ไม่ใช่แค่ client

**📋 LOG.md — append entry หลัง Gate ผ่าน (หรือ SKIP):**
```
## [NNN] YYYY-MM-DD | _start — Phase 4: Auth & Security
**Type**: Phase
**Skill Used**: _start + 08-auth-guard
**Action**: ตั้งค่า auth layer [หรือ SKIPPED]
**Decisions**:
- Auth strategy: JWT cookie / NextAuth / SKIPPED — เหตุผล: [reason]
**Files Created**:
- src/middleware.ts — route protection
- src/lib/auth.ts — getSession, requireAuth, requireRole
- src/lib/jwt.ts — sign/verify token
- src/lib/permissions.ts — RBAC matrix
**Gate**: ✅ / ⏭️ SKIPPED
```
