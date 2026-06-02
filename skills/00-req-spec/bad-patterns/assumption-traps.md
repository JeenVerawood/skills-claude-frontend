# Assumption Traps — สิ่งที่ห้าม Assume โดยไม่ถาม

## ❌ Trap 1: "Admin ทำได้ทุกอย่าง" 

```
requirement บอก: "Admin จัดการ user ได้"
trap: assume admin ลบ admin คนอื่นได้ด้วย
ความจริง: มักมี super-admin tier หรือ self-delete protection

✅ ต้องถาม: Admin สามารถแก้ไข/ลบ admin คนอื่นได้ไหม?
```

---

## ❌ Trap 2: "ลบ = หายไปตลอด"

```
requirement บอก: "admin ลบสัตว์ได้"
trap: implement hard delete ทันที
ความจริง: ลูกค้ามักต้องการ undo หรือดู history ได้

✅ default: soft delete เสมอ + ถามว่าต้องการ restore ได้ไหม
```

---

## ❌ Trap 3: "Filter = simple WHERE clause"

```
requirement บอก: "กรองตามสี, พันธุ์, ราคา"
trap: implement แยก filter ได้ทีละอย่าง
ความจริง: user ต้องการ combine filter พร้อมกัน

✅ ต้องถาม: filter ใช้ AND หรือ OR ระหว่างเงื่อนไข?
  ตัวอย่าง: สุนัข + สีขาว = AND (เอาสุนัขสีขาว)
            สีขาว + สีดำ = OR (เอาสีใดสีหนึ่ง)
```

---

## ❌ Trap 4: "User profile = แก้ได้ทุก field"

```
requirement บอก: "แก้ไขโปรไฟล์ได้"
trap: ให้แก้ email ได้ด้วยโดยไม่ verify ใหม่
ความจริง: email เปลี่ยนต้องผ่าน verification flow แยก

✅ ต้องถาม: เปลี่ยน email ได้ไหม? ถ้าได้ ต้อง verify email ใหม่ไหม?
```

---

## ❌ Trap 5: "Image = แค่เก็บ URL"

```
requirement บอก: "สัตว์มีรูปภาพ 3 รูป"
trap: เก็บแค่ URL string ใน array
ความจริง: ต้องการ order, primary image, alt text, lazy load strategy

✅ ต้องถาม:
  - รูปไหนเป็น primary (thumbnail)?
  - เรียงลำดับรูปได้ไหม?
  - มี alt text ไหม? (SEO / accessibility)
```

---

## ❌ Trap 6: "Inquiry = แค่ส่ง message"

```
requirement บอก: "ส่ง Inquiry ถึงร้านได้"
trap: implement เป็น simple contact form
ความจริง: มักต้องการ thread (ถาม-ตอบ), status tracking, notification

✅ ต้องถาม:
  - user เห็น reply จาก admin ได้ไหม? ใน UI หรือแค่ email?
  - 1 inquiry มีหลาย message (thread) ได้ไหม?
  - user รู้ได้ยังไงว่า admin ตอบแล้ว? (email? in-app notification?)
```

---

## ❌ Trap 7: "Pagination = แค่แบ่งหน้า"

```
requirement บอก: "มี pagination"
trap: implement page number pagination
ความจริง: ถ้าข้อมูลเพิ่มระหว่างที่ user ดูอยู่ → page offset เพี้ยน

✅ default: cursor-based pagination
  ถาม: ต้องการ "หน้า 1, 2, 3..." แบบ jump ได้ไหม? (ถ้าใช่ → offset pagination)
```

---

## ❌ Trap 8: "Available = boolean เดียว"

```
requirement บอก: "มี flag available"
trap: implement as boolean available: true/false
ความจริง: "ไม่ available" มีหลายสาเหตุ (sold, reserved, pending, hidden)

✅ ต้องถาม: สัตว์ที่ไม่ available มีสถานะย่อยไหม?
  เช่น sold / reserved / temporarily_unavailable / hidden_by_admin
```

---

## ❌ Trap 9: "Search = LIKE query"

```
requirement ไม่ได้พูดถึง search
trap: ไม่ implement search เพราะ requirement ไม่ได้บอก
ความจริง: user คาดหวัง search เสมอใน shop ที่มีสินค้าเยอะ

✅ ต้องถาม: ต้องการ search bar ไหม? search อะไรได้บ้าง? (ชื่อ? พันธุ์? คำอธิบาย?)
```

---

## ❌ Trap 10: "Admin dashboard = แค่ตาราง"

```
requirement บอก: "admin ดู stats ได้"
trap: แสดงตัวเลขเดี่ยวๆ (จำนวน user: 100)
ความจริง: admin มักต้องการ trend (เพิ่มขึ้น/ลดลง) และ time filter

✅ ต้องถาม: stats ต้องการ time range filter ไหม? (วันนี้ / สัปดาห์นี้ / เดือนนี้?)
  ต้องการ chart/graph ไหม หรือแค่ตัวเลข?
```
