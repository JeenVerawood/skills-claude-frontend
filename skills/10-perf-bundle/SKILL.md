---
name: 10-perf-bundle
description: >
  Use this skill for any performance optimization question in Next.js: bundle size, code splitting,
  dynamic imports, image optimization, React Server Components optimization, reducing client JS,
  lazy loading, font optimization, Core Web Vitals, and Lighthouse improvements. Triggers include:
  "bundle ใหญ่", "โหลดช้า", "dynamic import", "lazy load", "code splitting", "optimize",
  "performance", "LCP", "CLS", "Core Web Vitals", "Lighthouse", "re-render มาก",
  "use client ครอบใหญ่", "RSC", "first load JS". Always use this skill before writing any
  dynamic import, image component, or performance-related optimization.
---

# 10-perf-bundle: Performance

## Operating Stance

- **Measure first, optimize second.** ห้าม optimize โดยไม่รู้ว่า bottleneck อยู่ที่ไหน — premature optimization = เสียเวลา + code ซับซ้อนขึ้นเปล่า
- **RSC เป็น default.** ทุก component เริ่มเป็น Server Component — `'use client'` ต้องมีเหตุผล และต้องอยู่ leaf component เล็กที่สุดเท่านั้น
- **Impact-first.** แก้ bottleneck ที่ใหญ่ที่สุดก่อน — อย่าเสียเวลา micro-optimize ของที่ impact น้อย

---

## When NOT to use

- **re-render เยอะจาก useState/useEffect** → ใช้ Skill 03 แทน (React patterns)
- **ปัญหา network/API ช้า** → ใช้ Skill 05 (parallel fetch, Suspense)
- **ปัญหา image ใน CSS background** → นอก scope ของ next/image

---

## Refuse to proceed ถ้า

```
❌ user ขอ optimize โดยไม่บอกว่า bottleneck คืออะไร → ถามก่อน หรือให้ run Lighthouse ก่อน
❌ user ขอใส่ dynamic() ทุก component → อธิบาย overhead แล้วเลือกเฉพาะ heavy components
❌ user ขอ useMemo ทุก value เพื่อ "performance" → อธิบาย premature optimization
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/rsc-optimization.md` | use client ครอบใหญ่, Server vs Client Component boundary |
| `references/bundle-splitting.md` | dynamic import, code splitting, bundle size |
| `references/image-font.md` | next/image, next/font, layout shift, LCP |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/performance-mistakes.md` | use client ผิดที่, img ธรรมดา, sequential fetch |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/dynamic-imports.md` | dynamic import pattern สำเร็จรูป |
| `templates/image-patterns.md` | next/image patterns ทุก use case |

---

## Output Format (บังคับทุกครั้ง)

1. **ระบุ bottleneck** (bundle size / render time / network / LCP / CLS)
2. แสดง Before/After code พร้อม **impact ที่คาดว่าจะได้**
3. **Performance Checklist** ท้ายคำตอบ

---

## Worked Example

**ปัญหา:** "หน้า dashboard โหลดช้า first load JS ใหญ่"

**วิธีตอบที่ถูกต้อง:**
```
Bottleneck: 'use client' ครอบ component ใหญ่เกินไป → ส่ง JS ทั้งก้อนไป browser

ตรวจก่อน: เปิด Chrome DevTools → Coverage tab → ดูว่า JS ไหน unused

❌ Pattern ผิด — 'use client' บน parent ใหญ่:
'use client' // ← ทำให้ DashboardPage + ทุก child เป็น Client Component
export default function DashboardPage() {
  const [tab, setTab] = useState('overview')
  return (
    <div>
      <HeavyChart />      {/* ← Chart ทั้งก้อนไปอยู่ใน bundle */}
      <DataTable />       {/* ← Table ทั้งก้อนไปอยู่ใน bundle */}
    </div>
  )
}

✅ Fix — แยก 'use client' ไปอยู่ leaf เล็กๆ + dynamic() สำหรับ heavy:
// DashboardPage.tsx — Server Component (ไม่มี 'use client')
import dynamic from 'next/dynamic'
import { TabSwitcher } from './TabSwitcher' // Client component เล็กๆ

const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <ChartSkeleton />,
})

export default function DashboardPage() {
  return (
    <div>
      <TabSwitcher />  {/* Client ขนาดเล็ก */}
      <HeavyChart />   {/* โหลดแยก เมื่อ component ถึง viewport */}
      <DataTable />    {/* Server Component — ไม่มี JS */}
    </div>
  )
}

Impact คาดการณ์: First Load JS ลด ~30-50% (Chart library ออกจาก initial bundle)
```

**Performance Checklist:**
```
□ 'use client' อยู่เฉพาะ leaf component ที่ต้องการจริงๆ
□ Heavy components (chart, editor, map) ใช้ dynamic() + loading skeleton
□ ทุก <img> ใช้ next/image (ไม่ใช่ <img> ธรรมดา)
□ next/font แทน @import หรือ <link> font
□ Parallel fetch แทน sequential await
```

---

## Behavioral Rules

```
[MEASURE-FIRST]   ถามถึง bottleneck ก่อน — ห้าม optimize โดย assumption
[IMPACT-FIRST]    แก้สิ่งที่ impact ใหญ่ก่อน — อย่า micro-optimize ของที่ impact น้อย
[RSC-DEFAULT]     Server Component คือ default — เพิ่ม 'use client' เมื่อจำเป็นเท่านั้น
[SHOW-BEFORE-AFTER] ทุก optimization ต้องมี Before/After code + impact คาดการณ์
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill 03 (React Patterns) → แก้ re-render ก่อน แล้วค่อย optimize bundle
          start-all-project Phase 8 → audit RSC boundary + dynamic imports ทั้ง project
ถัดไป:    verify-completeness → ตรวจ performance checklist เป็นส่วนหนึ่งของ final check
```
