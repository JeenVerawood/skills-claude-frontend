---
name: 05-ts-zod
description: >
  Use this skill for any TypeScript question in a Next.js project: type definitions, avoiding any,
  Zod validation, generic types, interface vs type, utility types, tsconfig setup, strict mode,
  runtime type safety, and writing .d.ts files. Triggers include: "ใช้ any", "type ยังไง",
  "interface vs type", "generic", "Partial Omit Pick", "Zod", "tsconfig", "strict mode",
  "type assertion", "runtime crash", "Promise<any>", "return type", "utility types",
  "type safe", "third-party types". Always use this skill before generating TypeScript interfaces,
  type definitions, Zod schemas, or tsconfig changes.
---

# 05-ts-zod: TypeScript

## Operating Stance

- **Type safety ไม่ใช่ ceremony.** ทุก type ที่เพิ่มต้องแก้ปัญหาจริง — ไม่ใช่เพิ่มเพื่อให้ดูมี type
- **Compile-time vs runtime.** แยกให้ชัดก่อนเสมอ — TypeScript interfaces หายไปตอน compile ส่วน Zod ทำงานตอน runtime
- **No `any` tolerance.** `any` คือ silent bug เสมอ — ถ้าจำเป็นจริงๆ ให้ใช้ `unknown` + type guard แทน

---

## When NOT to use

- **ปัญหาเป็น tsconfig paths** → อ่าน `../02-folder-types/references/tsconfig-paths.md` แทน
- **ปัญหาเป็น Zod สำหรับ API** → ใช้ Skill 05 ที่รวม Zod ใน context ของ Route Handler แล้ว
- **แค่อยากรู้ว่าจะวาง type ไว้ที่ไหน** → ใช้ Skill 01 ที่มี component rules

---

## Refuse to proceed ถ้า

```
❌ user ขอ type ที่ใช้ any ทุกที่ → ห้าม comply อธิบายว่าทำไม any อันตราย แล้วเสนอ unknown + guard
❌ user ขอ type assertion (as Type) โดยไม่มีเหตุผล → ถามว่าทำไม แล้วเสนอ type guard แทน
❌ user ขอเพิ่ม type แต่ไม่รู้ว่าปัญหาเกิด compile-time หรือ runtime → ถามก่อน
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/tsconfig-standard.md` | ถามเรื่อง tsconfig, strict mode, compiler options |
| `references/type-conventions.md` | ถามเรื่อง interface vs type, utility types, naming |
| `references/zod-patterns.md` | ถามเรื่อง runtime validation, Zod schema |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/type-mistakes.md` | any, type assertion, Promise<any>, missing return type |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/result-pattern.md` | type-safe async function สำเร็จรูป |
| `templates/zod-schemas.md` | Zod schema สำหรับ User, API response, form |

---

## Output Format (บังคับทุกครั้ง)

1. **ระบุ** ว่าปัญหาเกิด compile-time หรือ runtime
2. แสดง ❌ (type ที่ผิด) + อธิบาย **ว่าทำไมถึงอันตราย**
3. แสดง ✅ (type ที่ถูก) + อธิบาย
4. ถ้าเกี่ยวกับ runtime safety → แสดง Zod schema ด้วยเสมอ

---

## Worked Example

**ปัญหา:** "ได้ข้อมูลจาก API แล้ว TypeScript ไม่ error แต่ runtime crash"

**วิธีตอบที่ถูกต้อง:**
```
ปัญหาเกิด: Runtime — TypeScript เชื่อ type ที่เรากำหนด แต่ API อาจส่งมาต่างกัน

❌ Unsafe — อันตรายเพราะ: TypeScript เชื่อ cast โดยไม่ validate จริง
const res = await fetch('/api/user')
const user = await res.json() as User // ← ถ้า API ส่งมาผิด shape = runtime crash

✅ Safe — Zod validate ตอน runtime ก่อน use:
import { z } from 'zod'

const UserSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
})

const res = await fetch('/api/user')
const raw = await res.json()
const user = UserSchema.parse(raw) // ← throw ZodError ถ้า shape ผิด
// ต่อจากนี้ user เป็น User จริงๆ ทั้ง compile-time และ runtime
```

---

## Behavioral Rules

```
[NO-ANY]          ห้าม recommend any ไม่ว่ากรณีไหน — ใช้ unknown + guard แทน
[COMPILE-VS-RT]   ระบุ compile-time vs runtime ก่อนเสมอ
[ZOD-FOR-RUNTIME] ทุกครั้งที่ข้อมูลมาจาก external source → ต้องมี Zod schema
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill 01 → วาง type files ตาม structure ที่กำหนด
ถัดไป:    Skill 05 (API) → Zod schema ที่สร้างที่นี่ใช้ validate ใน Route Handler
          start-all-project Phase 2 → สร้าง src/types/ ทั้งหมดตาม spec
```
