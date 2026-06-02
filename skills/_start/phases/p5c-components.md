# Phase 5c — Component Generation: สร้างทุก Component จาก Spec

**Summary Phase 5b ก่อนเริ่ม** (Page Manifest ที่ผ่าน Gate — ทุก page สร้างแล้ว)

## จุดประสงค์

สร้างทุก UI component และ Feature component ที่ระบุใน spec ให้ครบ ก่อนไป Phase 6 (API)

---

## ⛔ MANDATORY PRE-SCAN — ทำก่อนทุกอย่าง ห้ามข้ามเด็ดขาด

**ก่อนสร้างไฟล์แรกและก่อน Step 1 ต้องทำสิ่งนี้ก่อน:**

```
1. อ่านทุก page.tsx ที่สร้างใน Phase 5b ทีละไฟล์ (ไม่ใช่แค่จำ)
   → ค้นหา: TODO comments "Phase 5c: ComponentName"
   → ค้นหา: JSX elements ที่เป็น PascalCase (<ComponentName .../>)
   → ค้นหา: import statements ที่ชี้ไปยังไฟล์ที่ยังไม่มีอยู่

2. อ่าน spec ซ้ำที่ section "Pages & Routes" และ "Key Business Logic"
   → บันทึก component ทุกตัวที่ถูกกล่าวถึงโดยตรง

3. รวม 2 แหล่ง → สร้าง MASTER COMPONENT LIST

4. ประกาศจำนวนชัดเจน:
   UI Components รวม: __
   Feature Components รวม: __
   TOTAL: __
```

**ห้ามไป Step 1 จนกว่าจะอ่านทุก page.tsx ครบและมีตัวเลข TOTAL ที่แน่นอน**

---

## ขั้นตอนบังคับ

### Step 1 — สร้าง Component Manifest จาก Spec

อ่าน spec sections: **Pages & Routes** + **Key Business Logic** + ผล PRE-SCAN จากขั้นตอนบน

สร้างตาราง Component Manifest แยกเป็น 2 กลุ่ม:

#### UI Components (ใช้ได้หลาย feature)
```
| Component | File Path | Client/Server | Props (เบื้องต้น) | Status |
|---|---|---|---|---|
| Button | src/components/ui/Button.tsx | server | variant, size, onClick? | ❌ |
| Modal | src/components/ui/Modal.tsx | client | isOpen, onClose, children | ❌ |
```

#### Feature Components (specific ต่อ feature)
```
| Component | File Path | Client/Server | Props (เบื้องต้น) | Status |
|---|---|---|---|---|
| DashboardStats | src/components/features/dashboard/DashboardStats.tsx | server | - | ❌ |
| ProductCard | src/components/features/products/ProductCard.tsx | server | product: Product | ❌ |
```

**กฎ**: ห้ามข้ามไป Step 2 จนกว่า Component Manifest จะครบ และจำนวน rows ต้องตรงกับ TOTAL จาก PRE-SCAN

---

### Step 2 — สร้าง UI Components ก่อน (Atomic Loop)

สร้างทุกไฟล์ใน `src/components/ui/` ตาม manifest — **ทีละตัว ครบก่อนไปตัวถัดไป**:

```
สำหรับแต่ละ UI component (วนซ้ำจนครบทุกตัว):
1. สร้างไฟล์ตาม path ใน manifest
2. ใส่ TypeScript interface สำหรับ props
3. ใส่ implementation พื้นฐานพร้อม Tailwind classes
4. ไม่ใส่ 'use client' ถ้าไม่จำเป็น (useState/useEffect/browser API)
5. อัปเดต status → ✅ ทันทีในตาราง Manifest
6. ยืนยัน: ไฟล์สร้างแล้วจริง → ถึงไป component ถัดไปได้

ห้ามข้าม component ใดๆ แม้จะ "น่าจะง่าย" — ทุกตัวต้องมีไฟล์จริง
```

**ตัวอย่างขั้นต่ำ:**
```tsx
// src/components/ui/Button.tsx
interface ButtonProps {
  children: React.ReactNode
  variant?: 'primary' | 'secondary' | 'ghost'
  size?: 'sm' | 'md' | 'lg'
  onClick?: () => void
  disabled?: boolean
  type?: 'button' | 'submit' | 'reset'
}

export function Button({ children, variant = 'primary', size = 'md', ...props }: ButtonProps) {
  // TODO: implement with cn() utility
  return <button {...props}>{children}</button>
}
```

---

### Step 3 — สร้าง Feature Components (Atomic Loop)

สร้างทุกไฟล์ใน `src/components/features/` ตาม manifest — **ทีละตัว ครบก่อนไปตัวถัดไป**:

```
สำหรับแต่ละ Feature component (วนซ้ำจนครบทุกตัว):
1. สร้าง directory: src/components/features/{feature-name}/
2. สร้างไฟล์ component พร้อม:
   - TypeScript interface สำหรับ props
   - Structure/layout ที่สอดคล้องกับ spec
   - TODO comments สำหรับ logic ที่ต้องทำใน Phase 6
   - ไม่มี hardcoded data — ใช้ props เท่านั้น
3. อัปเดต status → ✅ ทันทีในตาราง Manifest
4. ยืนยัน: ไฟล์สร้างแล้วจริง → ถึงไป component ถัดไปได้

ห้ามประกาศ Step 3 เสร็จจนกว่า Feature Component count ใน manifest จะเท่ากับ PRE-SCAN TOTAL
```

**ตัวอย่างขั้นต่ำ:**
```tsx
// src/components/features/dashboard/DashboardStats.tsx
interface DashboardStatsProps {
  stats: {
    totalOrders: number
    totalRevenue: number
    totalUsers: number
  }
}

export function DashboardStats({ stats }: DashboardStatsProps) {
  return (
    <div className="grid grid-cols-3 gap-4">
      {/* TODO Phase 6: wire up real data via TanStack Query */}
      <div>{stats.totalOrders}</div>
      <div>{stats.totalRevenue}</div>
      <div>{stats.totalUsers}</div>
    </div>
  )
}
```

---

### Step 3b — กำหนด Mockup Images (ถ้ามี component ที่ต้องการรูป)

**อ่าน spec section "Project Overview" เพื่อระบุ domain ของ project ก่อนเลือกรูป**

#### กฎ Mockup Image (ห้ามละเมิด)

```
[NO-UNSPLASH-ID]   ห้ามใช้ Unsplash direct photo URL เด็ดขาด
                   ❌ https://images.unsplash.com/photo-1583511655826-05700442b31b?w=600
                   เหตุผล: photo ID เปลี่ยน/ถูกลบได้ตลอด → 404 เสมอ

[USE-PLACEHOLD]    Primary placeholder: https://placehold.co/{W}x{H}
                   ✅ https://placehold.co/400x300          (สีเทา)
                   ✅ https://placehold.co/400x300/png      (png)

[USE-PICSUM-SEED]  Photorealistic placeholder: https://picsum.photos/seed/{keyword}/{W}/{H}
                   ✅ https://picsum.photos/seed/dog/400/300
                   ✅ https://picsum.photos/seed/food/600/400
                   seed = keyword ที่สื่อถึง domain → รูปสม่ำเสมอทุก reload

[DOMAIN-MATCH]     keyword ใน seed ต้องตรงกับ domain ของ spec — ห้ามใช้ generic "photo1", "img"
```

#### Domain → Seed Keyword Map

| ประเภท Project | Seed Keywords แนะนำ |
|---|---|
| ขายสัตว์ / pet shop | dog, cat, pet, puppy, kitten |
| อาหาร / ร้านอาหาร | food, restaurant, dish, meal, burger |
| อสังหาริมทรัพย์ | house, building, interior, architecture |
| แฟชั่น / เสื้อผ้า | fashion, clothes, shirt, dress |
| ท่องเที่ยว | travel, beach, mountain, hotel |
| เทคโนโลยี / SaaS | laptop, office, workspace, technology |
| ฟิตเนส / สุขภาพ | fitness, gym, workout, health |
| การศึกษา | education, study, book, classroom |
| รถยนต์ | car, vehicle, automobile |
| ธุรกิจทั่วไป | business, team, office, meeting |

#### ตัวอย่าง Component กับ Mockup Image ที่ถูกต้อง

```tsx
// ❌ ผิด — Unsplash direct ID → 404
<Image src="https://images.unsplash.com/photo-1583511655826-05700442b31b?w=600" ... />

// ✅ ถูก — picsum seed ตรงกับ domain (pet shop)
<Image
  src="https://picsum.photos/seed/dog/400/300"
  alt="สุนัขพันธุ์ดี"
  width={400}
  height={300}
/>

// ✅ ถูก — placehold.co พร้อม label
<Image
  src="https://placehold.co/400x300?text=Pet+Photo"
  alt="รูปสัตว์เลี้ยง"
  width={400}
  height={300}
/>
```

#### next.config.js — ต้อง allowlist domains ที่ใช้จริง

เพิ่มเฉพาะ domain ที่ component ใช้จริง:

```js
// next.config.js
images: {
  remotePatterns: [
    // เพิ่มเฉพาะถ้า component ใช้ picsum
    { protocol: 'https', hostname: 'picsum.photos' },
    // เพิ่มเฉพาะถ้า component ใช้ placehold.co
    { protocol: 'https', hostname: 'placehold.co' },
  ],
}
```

---

### Step 4 — อัปเดต Pages ให้ import Components

กลับไปแก้ไขทุก `page.tsx` จาก Phase 5b:
- แทนที่ `{/* TODO Phase 5c: ComponentName */}` ด้วย `<ComponentName />` จริงๆ
- ส่ง props placeholder (mock data ชั่วคราว หรือ empty object ที่ type-safe)

### Step 4.5 — ORPHAN SCAN (บังคับหลัง Step 4)

```
หลังอัปเดต pages ครบแล้ว ต้องสแกนซ้ำ:
1. อ่านทุก page.tsx อีกครั้ง
2. ค้นหา import statement ทุกบรรทัดที่ชี้ไปยัง components/
3. ตรวจสอบว่าทุก import resolve ได้จริง (ไฟล์มีอยู่)
4. ถ้าพบ import ที่ไฟล์ไม่มี → สร้างไฟล์นั้นทันที ห้ามปล่อยทิ้ง
5. ถ้าพบ TODO Phase 5c ที่ยังค้างอยู่ใน page.tsx → สร้าง component นั้นทันที

ผ่าน Step 4.5 = ไม่มี unresolved import ในทุก page.tsx
```

---

### Step 5 — Component Manifest Report

แสดง Component Manifest สุดท้าย:

```
## Components Created ✓

### UI Components (X ตัว)
| Component | File | Client | Done |
|---|---|---|---|
| Button | src/components/ui/Button.tsx | no | ✅ |
...

### Feature Components (X ตัว)
| Component | File | Feature | Done |
|---|---|---|---|
| DashboardStats | src/components/features/dashboard/... | dashboard | ✅ |
...

รวม: X UI + X Feature = X components ทั้งหมด
```

---

## 🚨 กฎห้ามละเมิด

```
[PRE-SCAN-FIRST]   ต้องอ่านทุก page.tsx จริงๆ ก่อน Step 1 ห้าม assume จาก spec เพียงอย่างเดียว
[COUNT-MATCH]      จำนวน component ที่สร้างจริงต้องเท่ากับ TOTAL จาก PRE-SCAN — ห้ามน้อยกว่า
[ATOMIC-LOOP]      สร้างทีละตัว อัปเดต Manifest ทันที — ห้าม batch skip
[EVERY-COMPONENT]  ทุก component ที่ page.tsx อ้างถึงต้องมีไฟล์จริง — ห้ามปล่อย TODO ค้าง
[ORPHAN-SCAN]      Step 4.5 บังคับเสมอ — ห้ามประกาศ 5c เสร็จก่อนผ่าน orphan scan
[NO-INLINE]        ห้ามเขียน component logic ใน page.tsx โดยตรง — แยกไฟล์เสมอ
[TYPE-SAFE]        ทุก component ต้องมี TypeScript interface สำหรับ props — ห้าม any
[IMPORT-CHECK]     หลัง Step 4 ให้ตรวจว่า import ใน page.tsx ทุกไฟล์ resolve ได้ — ไม่มี import error
[MANIFEST]         ต้องแสดง Component Manifest ก่อน Gate ทุกครั้ง
[NO-UNSPLASH-ID]   ห้ามใช้ URL รูปแบบ images.unsplash.com/photo-XXXXXXXXX — ใช้ picsum.photos/seed หรือ placehold.co แทน
[IMAGE-DOMAIN]     keyword/seed ของรูป mockup ต้องตรงกับ domain ของ project — ห้ามใช้ generic ที่ไม่เกี่ยวข้อง
[NEXT-CONFIG]      ถ้ามี external image URL ต้อง allowlist hostname ใน next.config.js ด้วย
```

---

**✅ GATE 5c:** 
- Component Manifest มี ✅ ทุก row
- จำนวน components สร้างจริง = TOTAL จาก PRE-SCAN
- ทุก page.tsx import components ได้จริง (ไม่มี missing import)
- Step 4.5 Orphan Scan ผ่านแล้ว — ไม่มี unresolved import

**📝 project.md — อัปเดตหลัง Gate ผ่าน:**
- Phase 5c Details → **Total** (X UI + X Feature = X components)
- บันทึกรายชื่อ UI Components และ Feature Components แบบย่อ
- Phase Progress Table: Phase 5c → ✅ Gate → ✅
