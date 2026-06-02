---
name: 07-state-data
description: >
  Use this skill whenever a developer asks about state management choices, Zustand setup, Redux vs
  Context vs useState, TanStack Query for server state, React Hook Form, URL state with
  useSearchParams, or Context re-render issues. Triggers include: "state management", "Zustand",
  "Redux", "Context re-render", "server state", "TanStack Query", "SWR", "React Hook Form",
  "URL state", "useSearchParams", "global state", "จะเก็บ state ที่ไหน", "over-engineering",
  "store ใหญ่". Always use this skill before recommending or implementing any state management.
---

# 07-state-data: State Management

## Operating Stance

- **Minimize state ก่อน.** default คือ "ไม่ต้องการ state management" — เพิ่มเมื่อพิสูจน์ได้ว่าจำเป็น
- **Decision Matrix บังคับ.** ห้ามเลือก tool ก่อนผ่าน decision matrix — เลือกผิดแก้ทีหลังแพง
- **Server state ≠ Client state.** ข้อมูลที่มาจาก API คือ server state → ใช้ TanStack Query ไม่ใช่ Zustand

---

## When NOT to use

- **ปัญหาเป็น fetch pattern** (parallel, waterfall) → ใช้ Skill 05 แทน
- **ปัญหาเป็น re-render จาก hook** ไม่ใช่ global state → ใช้ Skill 03 แทน
- **ปัญหาเป็น form validation เฉพาะ** → อยู่ใน scope นี้ แต่ถ้าถามเรื่อง Zod schema ด้วย → ต้องอ่าน Skill 04 ด้วย

---

## Refuse to proceed ถ้า

```
❌ user ขอ Zustand store สำหรับเก็บ API response → เสนอ TanStack Query แทน ห้ามสร้าง Zustand ให้
❌ user ขอ Redux โดยไม่มีเหตุผล → ถามว่าทำไม เสนอ Zustand แทนพร้อมเหตุผล
❌ ไม่รู้ว่า state นั้นเป็น server state หรือ client state → ถามก่อนเสมอ
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/decision-matrix.md` | **อ่านก่อนเสมอ** — ไม่ว่าจะถามเรื่องอะไร |
| `references/zustand-patterns.md` | ถามเรื่อง Zustand store, selector, persist |
| `references/tanstack-query.md` | ถามเรื่อง TanStack Query, server state, cache |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/state-mistakes.md` | over-engineering, server state ใน global store, Context re-render |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/zustand-store.md` | Zustand store สำเร็จรูป (UI + async action + persist) |
| `templates/tanstack-setup.md` | TanStack Query setup + hooks สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. **แสดง Decision Matrix** ก่อนเสมอ — ระบุว่า state นี้ควรอยู่ที่ไหน
2. แสดง complete implementation พร้อม TypeScript types
3. อธิบายว่าทำไมถึงเลือก tool นี้ (ไม่ใช่แค่ "เพราะดี")

---

## Decision Matrix (ต้องแสดงทุกครั้ง)

```
State ชนิดนี้คืออะไร?
│
├─ ข้อมูลจาก API / server?
│   └─ TanStack Query (useQuery, useMutation)
│       ห้ามใช้ Zustand/useState เก็บ API data
│
├─ UI state ที่ใช้ข้าม component หลายตัว?
│   └─ Zustand store (ขนาดเล็ก focused)
│
├─ UI state ใน component เดียว หรือ subtree เล็กๆ?
│   └─ useState / useReducer
│
├─ ข้อมูลที่ sync กับ URL (filter, page, sort)?
│   └─ useSearchParams (URL state)
│
└─ Form state?
    └─ React Hook Form + Zod resolver
```

---

## Worked Example

**คำถาม:** "จะเก็บ user profile และ filter ของตาราง products ไว้ที่ไหนดี?"

**วิธีตอบที่ถูกต้อง:**
```
ผ่าน Decision Matrix ก่อน:

user profile → มาจาก API GET /api/me
→ ✅ TanStack Query: useQuery({ queryKey: ['me'], queryFn: getMe })
→ ❌ ห้าม: const [user, setUser] = useState() แล้ว fetch เอง
   เหตุผล: TanStack จัดการ loading/error/cache/refetch ให้ — เขียนเองซ้ำซ้อน

product filter (search, category, page) → sync กับ URL
→ ✅ URL state: useSearchParams()
→ ❌ ห้าม: Zustand store สำหรับ filter
   เหตุผล: filter ใน URL = shareable link, browser back ทำงานถูก, ไม่ต้อง hydrate
```

---

## Behavioral Rules

```
[MATRIX-FIRST]    ต้องแสดง Decision Matrix ก่อนเสมอ — ห้ามเสนอ tool โดยไม่ผ่าน matrix
[NO-ZUSTAND-API]  ห้ามใช้ Zustand เก็บข้อมูลที่มาจาก API ทุกกรณี
[EXPLAIN-WHY]     ทุกครั้งที่เลือก tool ต้องบอก "ทำไม" ไม่ใช่แค่ "ใช้อันนี้"
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill 05 (API) → TanStack hooks ที่สร้างที่นี่ call routes จาก Skill 05
ถัดไป:    Skill 03 (React Patterns) → ถ้า re-render เยอะหลังตั้ง state → audit component
          start-all-project Phase 6 → สร้าง stores + hooks ทั้งหมดตาม spec
```
