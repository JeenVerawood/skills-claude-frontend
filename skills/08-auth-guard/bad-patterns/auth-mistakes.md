# Auth Anti-patterns

## ❌ Mistake 1: JWT ใน localStorage

```ts
// ❌ XSS ขโมย token ได้ง่ายมาก
localStorage.setItem('token', jwt)
```

✅ Fix: httpOnly cookie เท่านั้น (ดู `templates/login-route.md`)

---

## ❌ Mistake 2: Cookie ไม่ปลอดภัย

```ts
// ❌ ขาด attributes สำคัญ
res.cookies.set('token', jwt)
// JS อ่านได้, ส่งใน HTTP ได้, CSRF ได้
```

✅ Fix:

```ts
res.cookies.set('auth-token', jwt, {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'lax',
  maxAge: 60 * 60 * 24 * 7,
  path: '/',
})
```

---

## ❌ Mistake 3: Auth เช็คแค่ฝั่ง client

```tsx
// ❌ bypass ได้ด้วย URL ตรงหรือแก้ JS
function DashboardPage() {
  const { user } = useAuth()
  if (!user) return redirect('/login')  // client-side เท่านั้น!
  return <Dashboard />
}
```

✅ Fix: ต้องเช็คทั้ง Middleware + API route (ดู `references/auth-layers.md`)

---

## ❌ Mistake 4: API route ไม่เช็ค session

```ts
// ❌ เรียก API ตรงโดยไม่ login ได้
export async function DELETE(req: Request, { params }) {
  // proxy ไป backend โดยไม่เช็ค session ก่อน — ใครก็ลบได้!
  await fetch(`${process.env.API_BASE_URL}/users/${params.id}`, { method: 'DELETE' })
  return Response.json({ success: true })
}
```

✅ Fix: requireAuth() บรรทัดแรกเสมอ (ดู `templates/auth-helpers.md`)

---

## ❌ Mistake 5: Secret อยู่ใน NEXT_PUBLIC_

```bash
# ❌ ทุกคนเห็น secret ใน client bundle
NEXT_PUBLIC_JWT_SECRET=my-secret-key
NEXT_PUBLIC_DATABASE_URL=postgresql://...
```

✅ Fix: ลบ NEXT_PUBLIC_ prefix — secrets ต้องเป็น server-only
