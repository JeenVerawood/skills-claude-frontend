---
name: 00-req-spec
description: >
  ใช้ skill นี้ทุกครั้งที่ได้รับ requirement จากลูกค้า ก่อนเริ่ม setup project หรือเขียนโค้ด frontend ใดๆ
  Skill นี้ครอบคลุมเฉพาะ Frontend (Next.js + TypeScript + Tailwind) เท่านั้น
  ไม่ครอบคลุม backend, database schema, หรือ server infrastructure
  Skill นี้ต้องรันก่อน Skill 00 เสมอ เพราะถ้า requirement ไม่ชัด การ setup ก็จะผิดทิศตั้งแต่แรก
  Triggers: "ลูกค้าส่ง requirement มา", "มี spec ใหม่", "analyze requirement",
  "วิเคราะห์ requirement", "ลูกค้าต้องการ", "requirement มาแล้ว",
  "ช่วยดู requirement", "มี project ใหม่", "วิเคราะห์ frontend"
  ห้ามข้าม Skill นี้แม้ requirement จะดูครบแล้ว — ยังมีสิ่งที่ซ่อนอยู่เสมอ
  ห้ามใช้ Skill นี้กำหนด backend architecture, database schema, หรือ API implementation
  → ส่วนนั้นเป็นความรับผิดชอบของ backend team / skill อื่น
---

# 00-req-spec: Requirements Analyzer (Frontend)

## Operating Stance

- **Detective ไม่ใช่ advocate.** อ่าน requirement เหมือนคนที่กำลังหาช่องโหว่ — ไม่ใช่คนที่พยายามจะเห็นด้วย
- **Frontend-only lens.** ทุกสิ่งที่วิเคราะห์ผ่านกรองของ "ทำบน Next.js ได้ไหม?" — ถ้าไม่ได้ให้ระบุว่าเป็น backend concern
- **ห้าม assume เด็ดขาด.** สิ่งที่ requirement ไม่ได้พูดถึง = ยังไม่รู้ — ไม่ใช่ "น่าจะ X"

---

## When NOT to use

- **ไม่มี requirement เลย** → ขอ requirement ก่อน ห้ามสร้าง spec จากอากาศ
- **requirement เป็นแค่ชื่อ project** → ไม่เพียงพอ ถามกลับอย่างน้อย 3 คำถามหลักก่อน
- **ต้องการแค่ config หรือ fix bug** → ใช้ Skill 00 หรือ skill ที่ตรงกว่านี้แทน

---

## Refuse to proceed ถ้า

```
❌ ไม่มี requirement text ที่อ่านได้เลย → ขอก่อน
❌ requirement ระบุแค่ "สร้างเว็บ" โดยไม่มี feature ใดๆ → ถามหา feature list ก่อน
❌ มี 🔴 Blocker ที่ยังไม่ได้รับคำตอบ → ห้าม proceed Phase ถัดไปจนกว่าจะได้รับคำตอบ
```

---

## จุดประสงค์

แปลง requirement คร่าวๆ จากลูกค้า → Frontend Spec ที่ละเอียดพอให้เริ่มเขียน Next.js ได้เลย
โดยไม่ต้องเดา และไม่ต้องกลับไปถามลูกค้าซ้ำระหว่างทำงาน

**Scope ของ Skill นี้ (Frontend เท่านั้น):**
- หน้าและ route ทั้งหมด
- Component ที่ต้องสร้าง
- State management (Zustand / TanStack Query)
- UI/UX behavior (filter, sort, pagination, loading, error state)
- API contract ที่ frontend ต้องการ (แค่ interface — ไม่ใช่ implementation)
- Auth flow ฝั่ง frontend (redirect, guard, cookie handling)

**นอก Scope — ห้ามกำหนดในนี้ (ห้ามทำเด็ดขาด):**
- Database schema หรือ migration
- Backend business logic หรือ implementation
- Server infrastructure หรือ deployment

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/ — อ่านก่อนเริ่ม Phase ใดก็ตาม
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/question-bank.md` | Phase 1 — ดึงคำถามที่เหมาะกับ project type |
| `references/decision-defaults.md` | Phase 2 — ถ้าลูกค้าไม่ตอบ ใช้ default นี้ |

### bad-patterns/ — อ่านก่อน Phase 2 เสมอ
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/assumption-traps.md` | ก่อนสรุป spec — ป้องกันการ assume ผิด |

### templates/ — Phase 3: Output
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/spec-output.md` | สร้าง Technical Spec document |
| `templates/ambiguity-checklist.md` | สร้าง Ambiguity List ส่งลูกค้า |

---

## Output Format (บังคับทุกครั้ง)

### Phase 1 — Intake & Classify
- อ่าน requirement ทั้งหมด
- จำแนก project type (e-commerce, SaaS, content site, internal tool ฯลฯ)
- โหลดคำถามที่เหมาะกับ project type นั้น

### Phase 2 — Gap Analysis
- หา ambiguity ทุกจุด — สิ่งที่ requirement บอกไม่ครบ
- แยกเป็น 3 ระดับ:
  - 🔴 Blocker — ตัดสินใจไม่ได้เลยถ้าไม่รู้ (ต้องถามลูกค้า)
  - 🟡 Important — มีผลต่อ architecture (ถามถ้ามีเวลา)
  - 🟢 Minor — ตัดสินใจเองได้โดยใช้ default

### Phase 3 — Generate Frontend Spec
- สร้าง Frontend Spec ครบทุก section
- ทุก decision ต้องมี rationale ว่าทำไม
- ระบุเฉพาะ API contract ที่ frontend ต้องการ — ไม่ใช่ implementation

### Phase 4 — Ambiguity Report
- สร้าง Ambiguity List พร้อม suggested answers
- จัดกลุ่มให้ลูกค้าตอบได้ง่าย

### Phase 5 — Handoff
- บอก developer ว่า Skill ต่อไปคือ **Skill 00 (project-setup)**
- แนบ spec ที่สร้างไว้เพื่อส่งต่อ

---

## Worked Example

**Input requirement:**
> "ต้องการทำ dashboard แสดงยอดขาย มี login และดูรายงานได้"

**Gap Analysis Output:**
```
🔴 Blocker:
- ไม่รู้ว่ามี role กี่ระดับ (admin เห็นอะไรต่างกับ user ไหม?)
- ไม่รู้ว่า API สำหรับ data มาจากไหน (มี backend อยู่แล้ว หรือต้องสร้างใหม่?)

🟡 Important:
- Export รายงานได้ไหม? (PDF/Excel)
- Dashboard realtime หรือ refresh เป็นรอบ?

🟢 Minor:
- Color scheme → [ASSUMED] ใช้ neutral palette
- Language → [ASSUMED] ภาษาไทย
```

**Frontend Spec Output (ส่วนหนึ่ง):**
```
Pages & Routes:
/ (redirect → /dashboard ถ้า logged in, /login ถ้าไม่ได้ login)
/login — public, form login
/dashboard — protected (role: any), แสดง summary cards + chart
/dashboard/reports — protected (role: admin), ตาราง export

API Contract (frontend needs):
POST /api/auth/login → { token } + httpOnly cookie
GET  /api/dashboard/summary → { totalSales, totalOrders, period }
GET  /api/reports → { rows[], totalCount }
```

---

## กฎสำคัญ

```
ห้าม assume สิ่งที่ลูกค้าไม่ได้บอก → ถามหรือใช้ default พร้อมระบุว่า assume อะไร
ห้ามข้าม Blocker items → ต้องได้คำตอบก่อนถึงจะเขียน spec ได้
ห้ามสร้าง spec ที่ขัดแย้งกันเอง → ตรวจ consistency ก่อน output
ห้ามกำหนด database schema, backend logic, หรือ infrastructure → นอก scope
API ที่ระบุใน spec คือ "สิ่งที่ frontend ต้องการ" เท่านั้น — ไม่ใช่ blueprint ให้ backend implement
```

## Cross-skill Chain

```
ก่อนหน้า: ไม่มี — Skill นี้คือจุดเริ่มต้นเสมอ
ถัดไป:    Skill 00 (project-setup) → ส่ง spec ที่สร้างไว้เป็น input
```
