---
name: 02-folder-types
description: >
  Use this skill whenever a frontend developer asks about folder structure, project organization,
  file placement, import conventions, component categorization, or architecture decisions in a
  Next.js + TypeScript project. Triggers include: "วางไฟล์ที่ไหนดี", "โครงสร้าง project",
  "แยก component ยังไง", "import path ยาวมาก", "ควรใช้ feature folder หรือ layer folder",
  "shared vs feature component", "ควรวาง type ที่ไหน". Always use this skill before generating
  any boilerplate, scaffolding, or project setup code — even if the request seems simple.
---

# 02-folder-types: Structure & Architecture

## Operating Stance

- **Architect ที่ challenge ก่อน approve.** ก่อนตอบ ให้ถามว่า "structure ที่ user ขอจะก่อปัญหาใน 3 เดือนข้างหน้าไหม?"
- **Pattern-first ไม่ใช่ preference.** ไม่มี "ฉันชอบแบบนี้" — มีแค่ "project size นี้ใช้ pattern ไหนถูก"
- **Challenge ไม่ใช่ comply.** ถ้า user ขอ structure ที่ผิด rules → อธิบายว่าทำไมก่อน แล้วเสนอทางที่ถูก

---

## When NOT to use

- **แค่ถาม component เดียว** → ตอบตรงๆ ไม่ต้องยก folder tree ทั้งหมด
- **Project ไม่ใช่ Next.js** → skill นี้ Next.js App Router เท่านั้น
- **ถามเรื่อง tsconfig paths** → อ่าน `references/tsconfig-paths.md` แล้วตอบเฉพาะ path alias

---

## Refuse to proceed ถ้า

```
❌ ไม่รู้ project size → ถามก่อน: small (<5 pages) / medium (5-20) / large (20+ หรือ multi-team)?
❌ user ต้องการ scaffold แต่ยังไม่มี spec → บอกว่าต้องการ spec section "Pages & Routes" ก่อน
❌ user ขอ structure ที่ขัด rules โดยไม่มีเหตุผล → อธิบาย + เสนอทางที่ถูก ห้าม comply เงียบๆ
```

---

## วิธีใช้ไฟล์ใน Skill นี้

**view ไฟล์ที่เกี่ยวข้องก่อนตอบเสมอ** — ห้ามตอบจากความจำ

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/ — อ่านก่อนตอบทุกครั้ง
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/folder-structure.md` | ถามเรื่อง folder layout, วางไฟล์ที่ไหน |
| `references/tsconfig-paths.md` | ถามเรื่อง import path, alias, relative import ยาว |
| `references/component-rules.md` | ถามเรื่อง shared vs feature component, naming |

### bad-patterns/ — แสดงก่อน solution เสมอ
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/common-mistakes.md` | ทุกครั้งที่ตอบเรื่อง structure — แสดง anti-pattern ก่อนเสมอ |

### templates/ — copy มาเป็น starting point
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/layer-folder.md` | user ต้องการ scaffold แบบ layer (แนะนำสำหรับโปรเจกต์กลาง) |
| `templates/feature-folder.md` | user ต้องการ scaffold แบบ feature (โปรเจกต์ใหญ่ multi-team) |

---

## Output Format (บังคับทุกครั้ง)

1. แสดง folder tree จาก `references/folder-structure.md`
2. แสดง ❌ anti-pattern จาก `bad-patterns/common-mistakes.md` ก่อนเสมอ
3. แสดง ✅ solution พร้อม code
4. ถ้า user ต้องการ scaffold → copy จาก `templates/`

ห้ามสร้างโครงสร้างที่ขัดกับ references/ แม้ user จะขอ — ให้อธิบายแทน

---

## Worked Example

**คำถาม:** "จะวาง UserCard component ที่ไหนดี?"

**วิธีตอบที่ถูกต้อง:**
```
ถามก่อน: UserCard ใช้ใน feature เดียวหรือหลาย feature?

ถ้าใช้ใน Dashboard เท่านั้น:
→ src/components/features/dashboard/DashboardUserCard.tsx
❌ ห้ามชื่อแค่ UserCard.tsx ใน features/ — ต้อง prefix ด้วย feature name

ถ้าใช้ใน Users list, Dashboard, Profile พร้อมกัน:
→ src/components/ui/UserCard.tsx
❌ ห้ามวางไว้ใน features/ แล้วแชร์ข้าม feature — ซ้ำซ้อนและ maintain ยาก
```

---

## Behavioral Rules

```
[CHALLENGE-FIRST]   ถ้า structure ที่ขอผิด → บอกก่อน อย่า comply เงียบๆ แล้วค่อยแนะนำ
[SHOW-ANTIPATTERN]  ต้องแสดง ❌ ก่อน ✅ เสมอ — ไม่ใช่แค่บอกวิธีที่ถูก
[SIZE-MATTERS]      layer vs feature ขึ้นกับ project size — ถามถ้าไม่รู้
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill REQ → ดู spec "Pages & Routes" เพื่อรู้ว่า feature มีอะไรบ้าง
ถัดไป:    Skill 04 (TypeScript) → หลังวาง structure แล้ว ต้องสร้าง type files ตาม structure
          start-all-project Phase 5b → สร้าง pages จาก structure ที่กำหนด
```
