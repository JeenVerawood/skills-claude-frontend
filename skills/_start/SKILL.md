---
name: start-all-project
description: >
  Master orchestrator skill สำหรับสร้าง Next.js + TypeScript project ครบจบตั้งแต่ต้นจนจบ
  จาก requirement → production-ready codebase โดยอ้างอิง Skill REQ + 00-09 ทั้งหมด
  Triggers: "start-all-project", "สร้าง project ให้ครบ", "ทำ project ให้จบ",
  "build project from requirement", "สร้างจาก requirement จนเสร็จ", "ทำครบทุกอย่าง"
---

# Skill: start-all-project (Master Orchestrator)

## Operating Stance

- **Orchestrator ไม่ใช่ improviser.** ทำตาม Phase Map เท่านั้น — ห้ามตัดสินใจ shortcut เองแม้จะดูเร็วกว่า
- **Spec คือ source of truth.** ทุกการตัดสินใจ (pages, components, routes) ต้องอ้างอิง spec ที่ได้จาก REQ เสมอ
- **Gate บังคับ ไม่ใช่ optional.** Gate แต่ละ Phase ไม่ใช่ checklist ตกแต่ง — ถ้าไม่ผ่านคือยังไม่เสร็จจริง

---

## When NOT to use

- **ถามเรื่อง feature เดียว** (เช่น "ทำ auth", "ทำ API route") → ใช้ Skill ที่ตรงกว่า (07, 05 ฯลฯ)
- **Project มีอยู่แล้วและแค่ต้องการ fix** → ใช้ Skill เฉพาะทาง ไม่ต้องรัน Phase ทั้งหมด
- **ยังไม่มี requirement เลย** → รัน Skill REQ ก่อน ห้าม start-all-project โดยไม่มี spec

---

## Refuse to proceed ถ้า

```
❌ ไม่มี Frontend Spec จาก REQ → รัน Skill REQ ก่อน ห้ามเริ่ม Phase 1
❌ มี 🔴 Blocker ค้างอยู่ → หยุดทันที แสดง Blocker list รอคำตอบก่อน
❌ Phase N ไม่ผ่าน Gate → ห้าม proceed Phase N+1 ต้องแก้จน Gate ผ่านก่อน
```

---

## ขอบเขต (Frontend เท่านั้น)

```
[SCOPE]    ทุก skill ในระบบนี้สนใจเฉพาะ Frontend (Next.js + TypeScript + Tailwind)
[NO-DB]    ห้ามสร้างไฟล์ที่ใช้ db.*, prisma.*, mongoose.* หรือ ORM/database ใดๆ
[NO-BE]    ถ้า requirement มี backend → สร้างเฉพาะ frontend code ที่ fetch() ไป external API
[API]      Route Handlers และ Server Actions ทำหน้าที่ proxy/BFF เท่านั้น
```

## กฎบังคับ

```
[GATE]       ทุก Phase มี Gate — ต้องผ่านก่อนเริ่ม Phase ถัดไป ห้าม skip
[CONTEXT]    ก่อนทำ Phase ใดๆ ให้อ่าน phase file ของ Phase นั้นก่อนเสมอ
[BLOCKER]    🔴 Blocker ทุกจุด → หยุดทันที รอผู้ใช้ตอบ ห้าม assume แล้วเดินต่อ
[SKIP]       Phase ที่ไม่เกี่ยวกับ spec → ประกาศ SKIPPED + เหตุผล แล้วไป Phase ถัดไป
[SUMMARY]    ก่อนเริ่ม Phase ใหม่ทุกครั้ง → สรุปผล Phase ก่อนใน 3-5 bullet points
[MANIFEST]   Phase 5b และ 5c ต้องแสดง Manifest ก่อน Gate — ห้ามข้าม
[NO-STUB]    ห้ามสร้างไฟล์ที่ return null หรือ empty โดยไม่มี structure จริง
[VERIFY]     Phase 9 บังคับเสมอ — ห้ามประกาศ DONE ก่อนผ่าน verify-completeness
[LOG-UPDATE] สร้าง LOG.md ที่ root ก่อน Phase 0 → ใช้ templates/activity-log.md
             append entry ลง LOG.md ทันทีหลังทุก action (ไม่ใช่แค่ Gate)
             ห้าม batch ทีหลัง — LOG.md คือ real-time record ของทุกสิ่งที่ทำ
```

## LOG.md Protocol

LOG.md อยู่ที่ root ของ project (เดียวกับ CLAUDE.md) — ไม่ใช่ในโฟลเดอร์ .claude

**สร้างครั้งแรก**: ก่อน Phase 0 → อ่าน `templates/activity-log.md` แล้วสร้าง LOG.md ที่ root

**append entry ทันทีเมื่อ:**

| เหตุการณ์ | Entry Type | สิ่งที่ต้องบันทึก |
|-----------|-----------|-----------------|
| เริ่ม + จบ Phase | Phase | action, files created/modified, decisions, gate status |
| สร้างไฟล์กลุ่มใหญ่ | Phase (ใน entry เดียวกัน) | list ทุกไฟล์ที่สร้าง |
| พบ Bug แล้วแก้ | Bug Fix | trigger (error message), root cause, files modified, fix |
| user ขอแก้ไข | Feature/Fix | trigger, files modified, notes |
| ตัดสินใจสำคัญ | บันทึกใน Decisions ของ entry ปัจจุบัน | decision + เหตุผล |
| Phase SKIP | Phase | SKIPPED + เหตุผล |

**กฎ**:
- entry number `[NNN]` เรียงต่อเนื่องตลอด project ไม่ reset
- append ต่อท้ายเสมอ — ห้ามแก้ entry เก่า
- ถ้า context reset → อ่าน LOG.md ครบก่อน resume จาก entry ล่าสุด
- ทุก skill ที่แก้ไข project files → append entry ลง LOG.md ด้วย (ไม่ใช่แค่ _start)

---

## Phase Map

| # | Phase File | Skill | จุดประสงค์ | Condition |
|---|---|-------|-----------|-----------|
| 0 | `phases/p0-req.md` | REQ | วิเคราะห์ requirement → Frontend Spec | บังคับ |
| 1 | `phases/p1-setup.md` | 00 | Install + Config files + CLAUDE.md | บังคับ |
| 2 | `phases/p2-struct.md` | 01 + 04 | Folder Structure + TypeScript | บังคับ |
| 3 | `phases/p3-env.md` | 08 | Environment config + validation | บังคับ |
| 4 | `phases/p4-auth.md` | 07 | Auth & Security | เฉพาะถ้า spec มี login |
| 5 | `phases/p5-nextjs.md` | 02 | Next.js Core (rendering, caching) | บังคับ |
| **5b** | **`phases/p5b-pages.md`** | **02** | **สร้างทุก Page จาก spec** | **บังคับ** |
| **5c** | **`phases/p5c-components.md`** | **01+03** | **สร้างทุก Component จาก spec** | **บังคับ** |
| 6 | `phases/p6-api.md` | 05 + 06 | API Routes + State Management | บังคับ |
| 7 | `phases/p7-react.md` | 03 | React Patterns audit | บังคับ |
| 8 | `phases/p8-perf.md` | 09 | Performance optimization | บังคับ |
| **9** | **`verify-completeness`** | **verify** | **ตรวจสอบ completeness ทุกไฟล์** | **บังคับ** |

## วิธีทำงาน

เมื่อจะเริ่ม Phase N:
1. อ่าน phase file ของ Phase นั้นจาก `phases/pN-xxx.md`
2. อ่านเฉพาะไฟล์ที่ระบุใน phase file นั้น
3. ทำตามขั้นตอนใน phase file จนผ่าน Gate แล้วจึงไป Phase ถัดไป

## กฎ Phase 5b + 5c (ห้ามละเมิด)

```
Phase 5b ต้องทำก่อน Phase 6 เสมอ — ทุก page ใน spec ต้องมีไฟล์จริง
Phase 5c ต้องทำก่อน Phase 6 เสมอ — ทุก component ที่ page.tsx อ้างถึงต้องมีไฟล์จริง
ห้าม proceed Phase 6 ถ้า Page Manifest หรือ Component Manifest ยังมี ❌
```

## กฎบังคับเพิ่มเติมสำหรับ Phase 5c (ห้ามละเมิดเด็ดขาด)

```
[SCAN-PAGES-FIRST]   Phase 5c ต้องเริ่มด้วยการอ่านทุก page.tsx จริงๆ ก่อนเสมอ
                     ห้าม assume component list จาก spec เพียงอย่างเดียว
                     → spec อาจไม่ระบุ component ย่อย ต้องดูจาก page.tsx ด้วย

[COUNT-BEFORE-BUILD] ก่อนสร้างไฟล์แรกต้องมีตัวเลข TOTAL ที่แน่นอน
                     จำนวน components ที่สร้างจริงต้องเท่ากับ TOTAL — ห้ามน้อยกว่า

[ATOMIC-CREATION]    สร้างทีละตัว อัปเดต Manifest ทันที ห้าม batch skip
                     ห้ามพูดว่า "สร้างแล้ว" โดยไม่ได้สร้างจริง

[ORPHAN-SCAN]        หลังสร้างครบต้องสแกน imports ทุก page.tsx ซ้ำ
                     ห้ามประกาศ 5c DONE ก่อนผ่าน orphan scan

[NO-PHASE6-WITHOUT-5c-GATE]
                     ถ้า 5c ยังไม่ผ่าน Gate → ห้าม proceed Phase 6 ไม่ว่ากรณีใด
```

## Phase 9 — verify-completeness (บังคับตอนสุดท้าย)

```
หลัง Phase 8 ผ่าน → รัน  skills\verify-completeness\SKILL.md ทันที
verify-completeness จะ:
  1. อ่าน spec ซ้ำ
  2. สร้าง Expected File List ครบทุก category
  3. Trace ทุกไฟล์ — ตรวจทั้ง existence และ content
  4. แสดง Completeness Report
  5. Auto-fix ไฟล์ที่ขาด
ห้ามประกาศ DONE จนกว่า verify-completeness จะ PASS ✅
```

---

## Behavioral Continuity Rules

```
[PHASE-MEMORY]     เมื่อเริ่ม Phase ใหม่ → สรุปผล Phase ก่อนใน 3-5 bullets ก่อนเสมอ
[BLOCKER-PERSIST]  Blocker ที่ยังไม่ได้รับคำตอบ → ย้ำใน Phase ถัดไปด้วย อย่าลืม
[ONE-ITERATION]    ถ้า user revise Phase เดิมเกิน 2 ครั้ง → ถามว่าส่วนไหนที่ยังไม่ถูกต้อง
[NO-SILENT-DONE]   ห้ามประกาศ DONE แบบเงียบๆ — ต้องแสดง Final Checklist ทุกครั้ง
```

## Cross-skill Chain

```
ก่อนหน้า: Skill REQ → Frontend Spec เป็น input สำหรับ Phase 0
ระหว่างทาง: ทุก Phase อ้าง Skill ที่ตรงกัน (00, 01, 02, 03, 04, 05, 06, 07, 08, 09)
สุดท้าย: verify-completeness → PASS = project ready ✅
```
