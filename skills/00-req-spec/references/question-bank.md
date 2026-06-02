# Question Bank — คำถามจำแนกตาม Project Type

## วิธีใช้

1. อ่าน requirement → จำแนก project type
2. โหลดคำถามจาก section ที่ตรงกัน
3. กรองเฉพาะคำถามที่ยัง requirement ไม่ได้ตอบ
4. จัดระดับ 🔴 / 🟡 / 🟢 ก่อนถาม

---

## คำถาม Universal (ถามทุก project เสมอ)

### Data Mode — 🔴 Blocker ถามเป็นอันดับแรกก่อนข้ออื่นทุกครั้ง

```
Q: ต้องการ Mock Data หรือเชื่อม Backend API จริง?

Option A — Mock Data
  → Frontend พัฒนา UI ได้ทันทีโดยไม่รอ Backend
  → UI/UX ทำงานได้ปกติ แต่ข้อมูลเป็น fake
  → เหมาะ: ยังไม่มี Backend / Backend ยังไม่พร้อม / ต้องการ demo

Option B — Real API
  → เชื่อมต่อ Backend จริงทันที ไม่มี mock layer
  → ต้องการ API_BASE_URL และ Backend พร้อมรับ request

Option C — Mixed (default ถ้าไม่ตอบ)
  → เริ่มจาก Mock ก่อน แล้วเปลี่ยนเป็น Real โดยแค่ตั้ง env var
  → ไม่ต้องแก้โค้ดเมื่อ switch

ถ้าเลือก B หรือ C → ถามเพิ่ม:
  - Backend URL? (เช่น http://localhost:8000)
  - มี API docs / Swagger URL ไหม?
  - Auth format? (Bearer token / API key / cookie / ไม่มี)
  - endpoint ไหนพร้อมใช้แล้ว? ไหนยังต้องสร้าง?
```

### Auth & Security
```
- มี authentication ไหม? (yes/no)
- ถ้ามี: ใช้ email+password / social login / SSO?
- มี role-based access control ไหม? มีกี่ role? แต่ละ role ทำอะไรได้บ้าง?
- Session หมดอายุเมื่อไหร่? (refresh token strategy?)
- ต้องการ "remember me" ไหม?
```

### Data & Storage
```
- ข้อมูลอะไรที่ sensitive ที่สุด? (PII, payment, health?)
- ต้องการ soft delete หรือ hard delete?
- ต้องการ audit log (ใครทำอะไร เมื่อไหร่) ไหม?
- Data retention policy มีไหม? (ลบข้อมูลเก่าหลัง X วัน?)
```

### Performance & Scale
```
- คาดว่าจะมี user กี่คนใน 6 เดือนแรก?
- traffic peak เกิดตอนไหน? (always-on vs burst)
- ต้องการ real-time features ไหม? (chat, live update, notification)
```

### Media & Files
```
- มี file upload ไหม? ถ้ามี: ประเภทไฟล์อะไร? ขนาด max เท่าไหร่?
- รูปภาพเก็บที่ไหน? (local / S3 / Cloudinary / Uploadthing?)
- ต้องการ image resize/optimization อัตโนมัติไหม?
```

### Error & UX
```
- ถ้า API error ให้แสดงอะไรกับ user? (toast / inline error / redirect?)
- ต้องการ loading skeleton ไหม?
- ต้องรองรับ offline mode ไหม?
- รองรับหลาย language ไหม? (i18n)
```

---

## คำถามเฉพาะ E-Commerce / Pet Shop

### สินค้า & Inventory
```
- stock จำกัดไหม? ถ้าสินค้าหมด ทำอะไร? (hide / show "out of stock" / waitlist?)
- ราคาเปลี่ยนบ่อยไหม? มี discount / coupon ไหม?
- มี variant ไหม? (เช่น สี, ไซส์ — ต่างจาก attribute ทั่วไป)
- ต้องการ product comparison ไหม?
```

### Cart & Checkout
```
- มี shopping cart ไหม? หรือแค่ inquiry (ติดต่อเพื่อซื้อ)?
- ถ้ามี cart: guest checkout ได้ไหม หรือต้อง login ก่อน?
- payment gateway ใช้อะไร? (Omise, PromptPay, Stripe?)
- ต้องการ order tracking ไหม?
```

### Review & Social
```
- user สามารถ review สินค้าได้ไหม?
- มี rating system ไหม? (ดาว?)
- share to social media ไหม?
```

---

## คำถามเฉพาะ SaaS / Internal Tool

### Billing
```
- มี subscription plan ไหม? (free / pro / enterprise?)
- billing cycle: รายเดือน / รายปี?
- ต้องการ usage-based billing ไหม? (จ่ายตาม API call เป็นต้น)
```

### Team & Organization
```
- user สามารถอยู่ใน organization ได้ไหม?
- organization มี sub-role ไหม? (owner, admin, member, viewer)
- invite system ทำงานยังไง? (email invite / join link?)
```

### Integration
```
- ต้องการ webhook ไหม?
- ต้องการ API key สำหรับ third-party integration ไหม?
- export ข้อมูลเป็น CSV / PDF ได้ไหม?
```

---

## คำถามเฉพาะ Content / Blog / CMS

### Content Structure
```
- ใครเป็นคน create content? (admin only / all users / curated authors?)
- มี draft / published state ไหม?
- ต้องการ scheduled publish ไหม? (post at specific time)
- มี version history ไหม? (ย้อนกลับไป revision เก่าได้?)
```

### SEO & Discovery
```
- SEO สำคัญแค่ไหน? (ต้องการ SSR / SSG ไหม?)
- มี sitemap auto-generate ไหม?
- ต้องการ custom meta per page ไหม?
- มี tag / category system ไหม?
```

---

## คำถาม Frontend-Specific (ถามทุก project ที่มี UI)

### Design
```
- มี Figma / design file ไหม?
- มี brand guideline หรือ color palette แล้วหรือยัง?
- reference site ที่ชอบ style คืออะไร?
- dark mode ต้องการไหม?
```

### Responsive
```
- ต้องรองรับ mobile ไหม? ระดับไหน? (responsive only / mobile-first / PWA / native app?)
- ขนาด screen ที่สำคัญที่สุดคือ? (desktop primary? mobile primary?)
```

### Accessibility
```
- มี requirement เรื่อง accessibility ไหม? (WCAG level?)
- ต้องรองรับ screen reader ไหม?
```
