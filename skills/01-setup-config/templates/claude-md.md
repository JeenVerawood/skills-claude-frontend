# Template: CLAUDE.md (สร้างที่ root ของ project)

```markdown
# CLAUDE.md

> คู่มือสำหรับ Claude Code — อ่านไฟล์นี้ก่อนทำงานทุกครั้ง

## Skills Location

Skills อยู่ที่ `./skills/` — อ่าน SKILL.md ที่ตรงกับ task ก่อนตอบเสมอ

## Skill Routing

| เมื่อต้องการ | อ่าน Skill |
|---|---|
| เริ่ม project ใหม่ / setup | `skills/01-setup-config/SKILL.md` |
| วาง folder / import path | `skills/02-folder-types/SKILL.md` |
| cache / hydration / middleware | `skills/03-nextjs-render/SKILL.md` |
| hook / useEffect / re-render | `skills/04-react-hooks/SKILL.md` |
| TypeScript / Zod / types | `skills/05-ts-zod/SKILL.md` |
| API route / fetch / server action | `skills/06-api-routes/SKILL.md` |
| state / Zustand / TanStack Query | `skills/07-state-data/SKILL.md` |
| auth / cookie / permission | `skills/08-auth-guard/SKILL.md` |
| env / .env / secret | `skills/09-env-config/SKILL.md` |
| performance / bundle / image | `skills/10-perf-bundle/SKILL.md` |

## Skill Chaining Rules

### เริ่มต้น project ใหม่
อ่านทั้งหมดตามลำดับ:
1. `skills/01-setup-config/SKILL.md` ← เริ่มที่นี่

### สร้าง API endpoint ใหม่
อ่านทั้งสองตามลำดับ:
1. `skills/06-api-routes/SKILL.md`
2. `skills/08-auth-guard/SKILL.md`
(API ทุกตัวต้องมี auth — ห้ามข้าม)

### สร้าง component ใหม่
อ่านทั้งสองตามลำดับ:
1. `skills/02-folder-types/SKILL.md`
2. `skills/04-react-hooks/SKILL.md`

## Project-specific Rules

- ภาษาใน comment และ commit message: ไทย + อังกฤษได้
- Import: ใช้ path alias `@/*` เสมอ ห้าม relative import ลึกเกิน 2 ชั้น
- Env: ห้ามใช้ `process.env.X` ตรงๆ — import จาก `@/lib/env` เสมอ
- Function ใน custom hook ที่ export ออกไป: ต้อง useCallback ทุกตัว
- ESLint: ห้าม eslint-disable เพื่อซ่อนปัญหา — แก้ root cause เสมอ

## Stack

- Framework: Next.js 14+ App Router
- Language: TypeScript (strict mode)
- Styling: Tailwind CSS
- State: Zustand (UI) + TanStack Query (server state)
- Form: React Hook Form + Zod
- DB: [ระบุเมื่อตัดสินใจ]
- Auth: [ระบุเมื่อตัดสินใจ]
```
