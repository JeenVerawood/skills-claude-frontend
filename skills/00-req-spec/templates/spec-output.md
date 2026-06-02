# Technical Spec Template

## วิธีใช้

copy template นี้ แล้วกรอกข้อมูลจาก requirement
ทุก section ที่มี `[TODO]` = ต้องกรอก ห้ามปล่อยว่าง
ทุก `[ASSUMED]` = ตัดสินใจเองโดยใช้ decision-defaults.md

---

# Technical Spec: [ชื่อ Project]

> Version: 1.0
> สร้างเมื่อ: [วันที่]
> สถานะ: Draft / Confirmed

---

## 1. Project Overview

```
ชื่อ project:   [TODO]
ประเภท:        [e-commerce / SaaS / content / internal tool]
เป้าหมาย:      [TODO — อธิบาย 1-2 ประโยค ว่า project นี้แก้ปัญหาอะไร]
กลุ่มเป้าหมาย: [TODO — ใครใช้ และใช้เพื่ออะไร]
Timeline:      [TODO — MVP ภายในเมื่อไหร่?]
```

---

## 2. Tech Stack

```
Frontend:  [TODO — Next.js 14 / React / etc.]
Backend:   [TODO — Express / Next.js API / Fastify / etc.]
Database:  [TODO — PostgreSQL / MySQL / MongoDB / etc.]
ORM:       [TODO — Prisma / Drizzle / Mongoose / etc.]
Auth:      [TODO — JWT / NextAuth / Clerk / etc.]
Storage:   [TODO — Uploadthing / S3 / Cloudinary]
Email:     [TODO — Resend / SendGrid / etc.]
Hosting:   [TODO — Vercel / Railway / VPS / etc.]
```

---

## 3. User Roles & Permissions

```
Role: [ชื่อ role 1]
  ทำได้:
    - [action 1]
    - [action 2]
  ห้าม:
    - [action ที่ห้าม]

Role: [ชื่อ role 2]
  ทำได้:
    - [action 1]
```

---

## 4. Data Schema (เบื้องต้น)

สร้าง 1 block ต่อ 1 entity หลัก

```prisma
model [EntityName] {
  id        String   @id @default(cuid())
  // [TODO: fields ทั้งหมด]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  deletedAt DateTime? // soft delete
}
```

**Relations:**
```
[Entity A] → [Entity B]: [one-to-many / many-to-many / one-to-one]
เหตุผล: [TODO]
```

---

## 5. API Integration Mode

```
Data Mode:     [Mock / Real API / Mixed]
API Base URL:  [TODO ถ้า Real/Mixed — เช่น http://localhost:8000]
API Docs URL:  [TODO ถ้ามี Swagger/Postman — ใส่ link]
Auth Format:   [Bearer token / cookie / API key / ไม่มี]
```

---

## 6. API Endpoints (Frontend → Backend)

Format ต่อ endpoint:
```
[METHOD] /backend-path
Frontend Route: /api/[next-route]   ← Next.js proxy route
Auth:    [public / user / role]
Input:   { field: type }
Output:  { field: type }
Mock:    yes / no
```

### Auth
```
POST /auth/login
Frontend Route: /api/auth/login
Auth:    public
Input:   { email, password }
Output:  { user } + httpOnly cookie
Mock:    yes

POST /auth/logout
Frontend Route: /api/auth/logout
Auth:    Bearer token
Output:  { success: true }
Mock:    yes

GET  /auth/me
Frontend Route: /api/auth/me
Auth:    Bearer token
Output:  { user }
Mock:    yes
```

### [Feature Group]
```
[TODO: ใส่ endpoints ของ feature นี้]
```

---

## 6b. Backend Connection Guide

> สร้างเฉพาะเมื่อ Data Mode = Real API หรือ Mixed
> Developer ใช้ checklist นี้เพื่อเชื่อม Backend ทีละ endpoint

### Step 1 — ตั้งค่า .env.local

```bash
API_BASE_URL=http://localhost:8000      # ← URL ของ Backend server
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
# [env vars อื่นๆ เช่น JWT_SECRET, API_KEY]
```

เมื่อตั้งค่า `API_BASE_URL` แล้ว mock data จะหายอัตโนมัติ (Mixed mode)

### Step 2 — Endpoint Checklist

เชื่อมทีละ endpoint แล้วทำเครื่องหมาย:

| # | Next.js Route | Backend Path | Method | Auth Required | สถานะ |
|---|---|---|---|---|---|
| 1 | /api/auth/login | /auth/login | POST | ไม่ | ⬜ |
| 2 | /api/auth/logout | /auth/logout | POST | Bearer | ⬜ |
| 3 | /api/auth/me | /auth/me | GET | Bearer | ⬜ |
| [N] | [TODO] | [TODO] | [TODO] | [TODO] | ⬜ |

_⬜ ยังไม่เชื่อม / ✅ เชื่อมแล้ว ทดสอบผ่าน / ❌ error_

### Step 3 — Response Format ที่ Backend ต้องส่ง

Frontend คาดหวัง format นี้จากทุก endpoint:

```ts
// Success response
{ "data": T, "message"?: string }

// Error response
{ "error": string, "code"?: string }
```

ถ้า Backend ใช้ format อื่น → แก้ mapping ที่ `src/lib/api/client.ts`

---

## 6. Pages & Routes

```
/ (Home)
  - [component หลักที่ต้องการ]
  - Data fetch: [API ที่ต้อง call]
  - Auth required: no

/[path] ([ชื่อหน้า])
  - [TODO]
```

---

## 7. Key Business Logic

อธิบาย logic ที่ซับซ้อน ไม่ใช่แค่ CRUD

```
[ชื่อ logic]: 
  trigger: [เมื่อไหร่]
  steps:
    1. [TODO]
    2. [TODO]
  edge cases:
    - [TODO]
```

---

## 8. Decisions & Rationale

| # | หัวข้อ | ตัดสินใจว่า | เหตุผล | Status |
|---|---|---|---|---|
| D1 | [TODO] | [TODO] | [TODO] | Confirmed / Assumed |
| D2 | [TODO] | [TODO] | [TODO] | Confirmed / Assumed |

---

## 9. Out of Scope (MVP)

สิ่งเหล่านี้ **ไม่ทำ** ใน phase นี้ เพื่อให้ deliver ได้เร็ว

```
- [feature ที่เลื่อนออกไป]
- [feature ที่เลื่อนออกไป]
```

---

## 10. Open Questions

รอคำตอบจากลูกค้า ยังทำ spec ส่วนนี้ไม่ได้

| # | คำถาม | ผลกระทบถ้าไม่รู้ | ด่วน? |
|---|---|---|---|
| Q1 | [TODO] | [TODO] | 🔴 / 🟡 / 🟢 |
