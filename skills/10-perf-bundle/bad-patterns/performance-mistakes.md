# Performance Anti-patterns

## ❌ Mistake 1: 'use client' ครอบ component ใหญ่

```tsx
// ❌ ทำให้ทั้ง tree กลายเป็น client bundle
'use client'
export default function DashboardPage() {
  const [tab, setTab] = useState('overview')
  return (
    <div>
      <TabBar tab={tab} onChange={setTab} />
      <StatsSection />    {/* ไม่ต้องการ client */}
      <ChartSection />    {/* ไม่ต้องการ client */}
      <TableSection />    {/* ไม่ต้องการ client */}
    </div>
  )
}
```

✅ Fix: แยก client logic เป็น leaf component (ดู `references/rsc-optimization.md`)

---

## ❌ Mistake 2: import library ใหญ่ทั้ง library

```ts
// ❌ import ทั้ง lodash (~70kb gzipped)
import _ from 'lodash'
const result = _.debounce(fn, 300)

// ❌ import ทั้ง date-fns
import * as dateFns from 'date-fns'
```

✅ Fix: tree-shake imports

```ts
import { debounce } from 'lodash-es'        // เฉพาะที่ใช้
import { format } from 'date-fns'           // เฉพาะที่ใช้
```

---

## ❌ Mistake 3: ใช้ `<img>` แทน `<Image />`

```tsx
// ❌ ไม่ optimize, ไม่ lazy load, ไม่ prevent layout shift
<img src="/hero.jpg" alt="Hero" />
```

✅ Fix: next/image เสมอ (ดู `templates/image-patterns.md`)

---

## ❌ Mistake 4: Heavy component ไม่ใช้ dynamic import

```tsx
// ❌ โหลดทุกครั้งแม้ไม่ได้ใช้
import { HeavyChart } from '@components/HeavyChart'    // +200kb
import { PdfViewer } from '@components/PdfViewer'      // +300kb
```

✅ Fix: dynamic import (ดู `templates/dynamic-imports.md`)

---

## ❌ Mistake 5: Google Fonts ด้วย `<link>`

```html
<!-- ❌ extra network request, layout shift (CLS) -->
<link href="https://fonts.googleapis.com/css2?family=Inter" rel="stylesheet" />
```

✅ Fix: next/font (ดู `references/image-font.md`)
