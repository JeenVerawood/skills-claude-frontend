# Path Alias มาตรฐาน

## tsconfig.json

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

`@/*` map ไปที่ root ของ project โดยตรง (ไม่มี `src/`)

## กฎการ import

- relative import ลึกเกิน 2 ชั้น → ต้องใช้ `@/` alias แทน
- `import type` สำหรับ type-only imports เสมอ

## ตัวอย่าง

```ts
// ✅ ถูก — ใช้ @/ จาก root
import { cn, formatCurrency } from '@/lib/utils'
import { connectDB } from '@/lib/db'
import { auth } from '@/lib/auth'
import { mockRooms, mockBills } from '@/lib/mockData'
import { StatCard } from '@/components/StatCard'
import { BillsClient } from '@/components/BillsClient'
import type { Room, Bill } from '@/types'

// ❌ ผิด — relative import ลึกเกิน 2 ชั้น
import { Button } from '../../../components/Button'
import type { Room } from '../../types/index'

// ❌ ผิด — ไม่มี src/ ใน project นี้
import { db } from '@/src/lib/db'
```
