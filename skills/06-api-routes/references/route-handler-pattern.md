# Route Handler Pattern (Frontend Proxy)

> Route Handler ใน Next.js ทำหน้าที่เป็น **BFF proxy** ไปยัง external backend API
> ห้าม call database โดยตรง — ทุก business logic อยู่ที่ backend

## โครงสร้างมาตรฐาน (ทุก handler ต้องมีครบ)

```
1. Auth check     → 401 ถ้าไม่มี session
2. Input validate → 400 ถ้า validation ไม่ผ่าน (Zod safeParse)
3. Proxy request  → fetch() ไป external backend API พร้อม auth header
4. Return         → forward response จาก backend กลับไปให้ client
5. Error catch    → 500 + log เสมอ
```

## HTTP Status Code Convention

```
200 → GET/PATCH/DELETE สำเร็จ
201 → POST สำเร็จ (สร้างใหม่)
400 → Validation failed (frontend validation)
401 → ไม่มี session
403 → มี session แต่ไม่มีสิทธิ์
404 → ไม่พบ resource (forward จาก backend)
409 → Conflict (forward จาก backend)
500 → Internal server error / network error
```

## Response Format มาตรฐาน

```ts
// Success
{ data: T, message?: string }

// Error
{ error: string, details?: unknown }
```

→ ดู template สำเร็จรูปใน `templates/route-handler.md`
