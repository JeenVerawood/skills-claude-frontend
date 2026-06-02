# Phase 5b — Page Generation: สร้างทุก Page จาก Spec

**Summary Phase 5 ก่อนเริ่ม** (rendering strategy ต่อ route ที่กำหนดไว้)

## จุดประสงค์

สร้างไฟล์ `page.tsx` ทุกไฟล์ที่ระบุใน spec section "Pages & Routes" ให้ครบก่อนไป Phase 6

---

## ขั้นตอนบังคับ

### Step 1 — สร้าง Page Manifest จาก Spec

อ่าน spec section **"Pages & Routes"** แล้วสร้างตาราง:

```
| Route | File Path | Auth Required | Data Fetch | Status |
|---|---|---|---|---|
| / | src/app/page.tsx | no | [API] | ❌ ยังไม่สร้าง |
| /[path] | src/app/[path]/page.tsx | yes | [API] | ❌ ยังไม่สร้าง |
```

**กฎ**: ห้ามข้ามไป Step 2 จนกว่าตารางจะครบทุก route ใน spec

---

### Step 2 — สร้างทุก Page File

สร้างทีละ route ตามลำดับใน spec:

```
สำหรับแต่ละ route:
1. สร้าง directory: src/app/[route-path]/
2. สร้าง page.tsx พร้อม:
   - ชื่อ component ถูกต้อง (PascalCase + Page suffix)
   - rendering mode จาก Phase 5 (static / dynamic / ISR)
   - Metadata (export const metadata หรือ generateMetadata)
   - Skeleton structure สำหรับ layout ของหน้านั้น
   - TODO comments ระบุ component ที่ต้องสร้างใน Phase 5c
3. ถ้า route มี auth → import และใช้ auth guard จาก Phase 4
4. อัปเดต status ในตาราง → ✅ สร้างแล้ว
```

**ตัวอย่าง page.tsx ขั้นต่ำที่ยอมรับได้:**

```tsx
// src/app/dashboard/page.tsx
import { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Dashboard',
}

export default function DashboardPage() {
  return (
    <main>
      {/* TODO Phase 5c: DashboardHeader */}
      {/* TODO Phase 5c: DashboardStats */}
      {/* TODO Phase 5c: DashboardRecentActivity */}
    </main>
  )
}
```

---

### Step 3 — สร้าง Route Groups และ Layouts

สำหรับแต่ละ route group ใน spec:

```
□ (auth)/layout.tsx      — ถ้ามี auth routes
□ (dashboard)/layout.tsx — ถ้ามี dashboard routes
□ [dynamic]/page.tsx     — dynamic routes ทุกตัว
□ [...slug]/page.tsx     — catch-all routes ถ้ามีใน spec
```

---

### Step 4 — สร้าง Page Manifest Report

หลังสร้างครบ แสดง Page Manifest สุดท้าย:

```
## Pages Created ✓

| Route | File | Rendering | Auth |
|---|---|---|---|
| / | src/app/page.tsx | static | no |
| /dashboard | src/app/dashboard/page.tsx | dynamic | yes |
...

รวม: X pages สร้างแล้ว จาก X pages ใน spec
```

---

## 🚨 กฎห้ามละเมิด

```
[NO-SKIP]    ห้ามข้าม route ใดๆ ใน spec แม้จะ "simple" — สร้างทุกตัว
[NO-STUB]    ห้ามสร้าง page ที่ return null หรือ return <div/> เปล่าๆ โดยไม่มี TODO comments
[MANIFEST]   ต้องแสดง Page Manifest ก่อน Gate ทุกครั้ง
[MATCH-SPEC] ชื่อ file path ต้องตรงกับ route ใน spec — ห้าม rename โดยไม่แจ้ง
```

---

**✅ GATE 5b:** แสดง Page Manifest ที่มี ✅ ทุก row — ทุก page ใน spec ถูกสร้างจริง + ไม่มี ❌ ค้างอยู่

**📝 project.md — อัปเดตหลัง Gate ผ่าน:**
- Phase 5b Details → copy Page Manifest table (route, file, rendering mode, auth)
- บันทึกจำนวน: "X pages สร้างแล้ว จาก X pages ใน spec"
- Phase Progress Table: Phase 5b → ✅ Gate → ✅
