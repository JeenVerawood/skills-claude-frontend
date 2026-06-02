---
name: verify
description: >
  ตรวจสอบว่า project ที่ generate ออกมาครบถ้วนตาม spec หรือไม่
  อ่าน spec แล้ว trace ทุก page และ component ที่ควรมี เทียบกับไฟล์จริงใน codebase
  ถ้าพบไฟล์ขาด → แสดง gap report พร้อมสั่งสร้างไฟล์ที่หายไปทันที
  Triggers: "verify", "ตรวจสอบ", "ครบหรือยัง", "check completeness", "มีอะไรขาดไหม",
  "verify-completeness", "ตรวจว่าครบ", "สรุปสิ่งที่ generate"
---

# Skill: verify-completeness

## จุดประสงค์

ทำหน้าที่เป็น "outsider auditor" — อ่าน spec แล้วตรวจว่าทุกไฟล์ที่ควรมีถูกสร้างจริงหรือไม่
ถ้าขาด → แสดงรายการชัดเจนและสร้างไฟล์ที่ขาดทันที

---

## Operating Stance

- **Spec-first.** ไม่ดูโค้ดก่อน — อ่าน spec ก่อน แล้วค่อย verify
- **Outsider auditor.** อ่าน spec เหมือนคนที่ไม่เคยเห็น codebase นี้มาก่อน — ไม่มี bias ว่า "น่าจะสร้างไว้แล้ว"
- **Trace ทุกไฟล์.** ไม่ตัดสินจาก folder tree เท่านั้น — เปิดไฟล์จริงเพื่อยืนยัน content
- **Zero tolerance.** ไฟล์ขาด 1 ไฟล์ = INCOMPLETE — ห้ามประกาศ DONE ก่อนแก้ครบ
- **Auto-fix.** ถ้าพบ gap → สร้างไฟล์ที่ขาดทันที ไม่ใช่แค่รายงาน

---

## Workflow (ทำตามลำดับ ห้าม skip)

### Phase 1 — อ่าน Spec

อ่านไฟล์ spec (หาจาก CLAUDE.md หรือถามผู้ใช้ถ้าไม่แน่ใจ) โดยเน้น sections:

```
□ Section "Pages & Routes"         → รายชื่อ pages ทั้งหมดที่ควรมี
□ Section "Components"             → รายชื่อ components ถ้าระบุไว้
□ Section "API Endpoints"          → Route Handlers ที่ต้องมี
□ Section "State Management"       → Zustand stores / TanStack hooks
□ Section "User Roles & Permissions" → Auth guards ที่ต้องมี
```

สร้าง **Expected File List** — รายการไฟล์ทั้งหมดที่ spec กำหนด

---

### Phase 2 — สร้าง Expected File List

แปลง spec เป็นรายการไฟล์ที่คาดหวัง:

```
## Expected Files

### Pages (จาก spec section "Pages & Routes")
- [ ] src/app/page.tsx
- [ ] src/app/[route]/page.tsx
- [ ] src/app/[route]/loading.tsx
- [ ] src/app/[route]/error.tsx

### Layouts
- [ ] src/app/layout.tsx
- [ ] src/app/(auth)/layout.tsx   ← ถ้ามี auth group

### UI Components (จาก spec + component-rules.md)
- [ ] src/components/ui/Button.tsx
- [ ] src/components/ui/[...]

### Feature Components (จาก spec section "Pages & Routes")
- [ ] src/components/features/{feature}/{ComponentName}.tsx
- [ ] src/components/features/[...]

### API Routes (จาก spec section "API Endpoints")
- [ ] src/app/api/{endpoint}/route.ts
- [ ] src/app/api/[...]

### Hooks (จาก spec — TanStack Query hooks)
- [ ] src/hooks/use{Feature}.ts
- [ ] src/hooks/[...]

### Stores (จาก spec — Zustand)
- [ ] src/stores/{feature}Store.ts
- [ ] src/stores/[...]

### Types (จาก spec entities)
- [ ] src/types/{entity}.types.ts
- [ ] src/types/[...]

### Config & Boilerplate
- [ ] src/lib/env.ts
- [ ] src/lib/utils.ts
- [ ] src/types/index.ts
- [ ] .env.example
- [ ] CLAUDE.md
```

---

### Phase 3 — Trace: ตรวจทุกไฟล์

สำหรับทุกไฟล์ใน Expected File List:

```
1. ตรวจว่าไฟล์มีอยู่จริง (Read ไฟล์นั้น)
2. ถ้ามี → ตรวจ content ขั้นต่ำ:
   - Page: มี export default function ไหม? มี metadata ไหม?
   - Component: มี TypeScript interface props ไหม?
   - API Route: มี handler function ไหม? มี error handling ไหม?
   - Hook: export ถูก function ไหม?
3. อัปเดต status:
   ✅ = มีและ content ผ่าน
   ⚠️ = มีแต่ content ไม่ครบ (stub/empty)
   ❌ = ไม่มีไฟล์
```

### Phase 3b — IMPORT SCAN (บังคับ ทำหลัง Phase 3)

```
⚠️ Expected File List อาจไม่ครอบคลุมทุก component ที่สร้างจริงๆ
→ ต้องสแกน imports ด้วยเพื่อจับสิ่งที่หลุดจาก Expected List

ขั้นตอน:
1. อ่านทุก page.tsx และ layout.tsx ในโปรเจกต์ (ทุกไฟล์จริงๆ ไม่ใช่แค่จำ)
2. ดึง import statements ทุกบรรทัดที่ชี้ไปยัง:
   - @/components/
   - ../components/
   - ./components/
   - src/components/
3. สำหรับทุก import → ตรวจว่าไฟล์ปลายทางมีอยู่จริง
4. ถ้าพบ import ที่ไฟล์ไม่มี → เพิ่มในรายการ ❌ ทันที
5. ถ้าพบ {/* TODO Phase 5c: ... */} ที่ยังไม่ถูกแทนที่ → เพิ่มในรายการ ⚠️

ผล: component ที่หลุดจาก Expected List จะถูกจับได้ที่นี่
```

---

### Phase 4 — Completeness Report

แสดง report สรุป:

```
## Completeness Report

### Summary
| Category | Expected | Found ✅ | Stub ⚠️ | Missing ❌ |
|---|---|---|---|---|
| Pages | X | X | X | X |
| Layouts | X | X | X | X |
| UI Components | X | X | X | X |
| Feature Components | X | X | X | X |
| API Routes | X | X | X | X |
| Hooks | X | X | X | X |
| Stores | X | X | X | X |
| Types | X | X | X | X |
| Config | X | X | X | X |
| **TOTAL** | **X** | **X** | **X** | **X** |

### Status
[COMPLETE ✅] หรือ [INCOMPLETE ❌ — X files missing/stub]

### Missing Files (ถ้ามี)
❌ src/app/products/page.tsx — Page สำหรับ /products ไม่มีไฟล์
⚠️ src/components/ui/Button.tsx — มีแต่ return null (stub)
...

### Stub Files (ถ้ามี)
⚠️ [file path] — [อธิบายว่าขาดอะไร]
```

---

### Phase 5 — Auto-Fix: สร้างไฟล์ที่ขาด

**ถ้ามี ❌ หรือ ⚠️ → สร้างทันที ห้ามรอให้ผู้ใช้สั่ง**

```
สำหรับทุก ❌ (Missing):
  → สร้างไฟล์ด้วย implementation ขั้นต่ำที่ถูกต้อง
  → ไม่ใช่แค่ skeleton เปล่า — ต้องมี structure จริงจากที่เห็นใน spec

สำหรับทุก ⚠️ (Stub):
  → เปิดไฟล์ อ่าน implementation ปัจจุบัน
  → เพิ่มส่วนที่ขาด
```

หลัง auto-fix → run Phase 3 ซ้ำเฉพาะไฟล์ที่แก้ → update report

---

### Phase 6 — Final Verdict

```
## Final Verification

[PASS ✅] Project COMPLETE — ทุกไฟล์ใน spec ถูกสร้างและมี content จริง

หรือ

[FAIL ❌] ยังขาด X files — [รายการ]
→ กำลังสร้างไฟล์ที่ขาด...
```

**ห้ามประกาศ PASS จนกว่าทุกไฟล์จะ ✅**

**📝 project.md — อัปเดตหลัง Final Verdict:**
- Phase 9 Details → copy Completeness Report Summary table (Expected / Found ✅ / Missing ❌)
- บันทึก **Result**: PASS ✅ หรือ FAIL ❌ + จำนวนไฟล์ที่แก้
- Phase Progress Table: Phase 9 → ✅ Gate → ✅
- ถ้า PASS → เพิ่มบรรทัดสุดท้ายใน project.md:
  `**🎉 Project DONE** — verify-completeness PASS ✅ | [YYYY-MM-DD]`

---

## Worked Example

**Completeness Report ที่ดี:**
```
## Completeness Report

### Summary
| Category | Expected | Found ✅ | Stub ⚠️ | Missing ❌ |
|---|---|---|---|---|
| Pages | 5 | 3 | 1 | 1 |
| UI Components | 4 | 4 | 0 | 0 |
| Feature Components | 6 | 4 | 0 | 2 |
| API Routes | 3 | 3 | 0 | 0 |
| Hooks | 2 | 1 | 0 | 1 |
| **TOTAL** | **20** | **15** | **1** | **3** |

### Status: INCOMPLETE ❌ — 3 files missing, 1 stub

### Missing Files
❌ src/app/products/[id]/page.tsx
   → spec ระบุ route /products/[id] (Product Detail Page) แต่ไม่มีไฟล์
❌ src/components/features/products/ProductReviews.tsx
   → spec ระบุ component "ProductReviews" ใน Product Detail Page
❌ src/hooks/useCart.ts
   → spec ระบุ "cart state" แต่ hook ไม่ถูกสร้าง

### Stub Files
⚠️ src/app/checkout/page.tsx
   → มีไฟล์แต่ return <div>TODO</div> — ไม่มี structure จาก spec

→ กำลัง auto-fix ไฟล์ที่ขาดทั้ง 4 รายการ...
```

---

## กฎห้ามละเมิด

```
[SPEC-FIRST]    อ่าน spec ก่อนเสมอ — ห้าม assume ว่า "น่าจะครบแล้ว"
[NO-RUBBER]     ห้ามประกาศ COMPLETE โดยไม่ trace ทุกไฟล์จริงๆ
[AUTO-FIX]      พบ gap → สร้างทันที — ไม่ใช่แค่รายงานแล้วหยุด
[TRACE-CONTENT] ตรวจ content ด้วย ไม่ใช่แค่ตรวจว่าไฟล์มีอยู่
[CITE]          ทุก finding ต้องระบุ file path — ห้ามพูดลอยๆ
```

## Cross-skill Chain

```
ก่อนหน้า: start-all-project Phase 8 → รัน verify เป็น Phase 9 เสมอ
ผลลัพธ์:  PASS ✅ = project DONE / FAIL ❌ = auto-fix แล้วรัน verify ซ้ำ
```
