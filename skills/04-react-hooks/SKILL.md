---
name: 04-react-hooks
description: >
  Use this skill for any question about React hooks usage, component patterns, and common React
  mistakes in a Next.js + TypeScript project. Triggers include: "useEffect loop", "infinite loop",
  "stale closure", "dependency array", "prop drilling", "useState vs useRef", "useMemo",
  "useCallback", "re-render มากเกิน", "memory leak", "race condition", "key prop",
  "component ช้า", "hook ใช้ยังไง". Always use this skill before writing any React hook logic,
  component with state, or useEffect code.
---

# 04-react-hooks: React Patterns

## Operating Stance

- **Pattern matcher ก่อน prescriber.** ระบุ pattern ปัญหา (stale closure / infinite loop / unnecessary re-render) ก่อนเสนอ fix — ถ้าระบุไม่ได้แปลว่ายังไม่เข้าใจปัญหา
- **Minimal intervention.** เพิ่ม useMemo / useCallback เฉพาะเมื่อมีหลักฐานว่าจำเป็น — ไม่ใส่เพราะ "น่าจะดี"
- **Server Component ก่อน.** ทุก component เริ่มต้นเป็น Server Component — เพิ่ม `'use client'` เมื่อจำเป็นเท่านั้น

---

## When NOT to use

- **ปัญหาเป็น Next.js caching** → ใช้ Skill 02 แทน
- **ปัญหาเป็น state management tool** (Zustand, TanStack Query) → ใช้ Skill 06 แทน
- **component ช้าเพราะ bundle ใหญ่** → ใช้ Skill 09 แทน

---

## Refuse to proceed ถ้า

```
❌ user บอก "มี re-render เยอะ" โดยไม่บอก component ไหน → ขอ code snippet ก่อน
❌ user ขอให้ใส่ useMemo ทุก value → อธิบายว่า premature optimization ก่อน
❌ user ขอ useEffect แต่ไม่รู้ว่า dependency คืออะไร → อธิบาย mental model ก่อนเขียน code
```

---

## วิธีใช้ไฟล์ใน Skill นี้

**view ไฟล์ที่ตรงกับปัญหาก่อนตอบเสมอ** — ห้ามตอบจากความจำ

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/useeffect-rules.md` | ถามเรื่อง useEffect, dependency, cleanup |
| `references/state-vs-ref.md` | ถามเรื่อง useState vs useRef |
| `references/memo-callback.md` | ถามเรื่อง useMemo, useCallback, re-render |
| `references/context-pattern.md` | ถามเรื่อง prop drilling, Context |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/useeffect-mistakes.md` | infinite loop, stale closure, memory leak |
| `bad-patterns/rerender-mistakes.md` | re-render มากเกิน, key prop ผิด |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/async-effect.md` | ต้องการ async useEffect พร้อม cleanup |
| `templates/context-provider.md` | ต้องการ Context + custom hook สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. **ระบุ pattern** ปัญหา (stale closure / infinite loop / unnecessary re-render / missing cleanup)
2. แสดง ❌ code จาก `bad-patterns/` พร้อม comment อธิบาย **ว่าทำไมถึงเกิดปัญหา**
3. แสดง ✅ solution จาก `references/` พร้อม code
4. ถ้ามีหลาย solution → บอกเมื่อไหรใช้อะไร พร้อม tradeoff

---

## Worked Example

**ปัญหา:** "useEffect รัน infinite loop"

**วิธีตอบที่ถูกต้อง:**
```
Pattern: Object/Array reference ใน dependency array

❌ ทำให้ loop:
const [data, setData] = useState({ page: 1 })

useEffect(() => {
  fetchData(data)
}, [data]) // ← data เป็น object ใหม่ทุก render → loop ไม่หยุด

ทำไมถึงเกิด: React เปรียบ dependency ด้วย Object.is()
Object { page: 1 } !== Object { page: 1 } แม้ค่าเหมือนกัน

✅ Fix ตัวเลือก 1 — ใช้ primitive แทน object:
const [page, setPage] = useState(1)
useEffect(() => {
  fetchData(page)
}, [page]) // ← number เปรียบกัน stable

✅ Fix ตัวเลือก 2 — ถ้าต้องใช้ object จริงๆ ใช้ useRef:
const dataRef = useRef({ page: 1 })
```

---

## Behavioral Rules

```
[PATTERN-FIRST]    ระบุ pattern ก่อนเสมอ — ห้ามเสนอ fix โดยไม่รู้ว่าปัญหาคืออะไร
[NO-PREMATURE]     ห้าม recommend useMemo/useCallback โดยไม่มีหลักฐาน re-render problem
[SHOW-WHY]         อธิบายว่า ❌ code เกิดปัญหาเพราะอะไร ไม่ใช่แค่บอกว่าผิด
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: start-all-project Phase 5c → component ที่สร้างมาต้องผ่าน audit Phase 7
ถัดไป:    Skill 09 (Performance) → ถ้า re-render pattern ถูกแต่ยังช้า → ดู bundle/RSC
          Skill 06 (State) → ถ้าปัญหา re-render มาจาก global state
```
