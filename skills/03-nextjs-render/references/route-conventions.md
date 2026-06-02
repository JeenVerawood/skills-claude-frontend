# Route File Conventions

## ไฟล์ที่รองรับในแต่ละ route segment

```
app/dashboard/
├── page.tsx          ← UI หลัก (required)
├── layout.tsx        ← Persistent layout ครอบ children
├── loading.tsx       ← Suspense fallback (แสดงทันที)
├── error.tsx         ← Error boundary (ต้อง 'use client'!)
├── not-found.tsx     ← 404 สำหรับ route นี้
└── template.tsx      ← เหมือน layout แต่ re-mount ทุก navigation
```

## error.tsx — ต้อง 'use client' เสมอ

```tsx
'use client'
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>เกิดข้อผิดพลาด</h2>
      <p>{error.message}</p>
      <button onClick={reset}>ลองใหม่</button>
    </div>
  )
}
```

## generateStaticParams — ต้อง return ครบ

```ts
export async function generateStaticParams() {
  const products = await fetchAllProducts()
  return products.map((p) => ({ id: String(p.id) }))
}

// กำหนด behavior สำหรับ id ที่ไม่ได้ pre-render
export const dynamicParams = true   // render on-demand (default)
// export const dynamicParams = false // 404 ทันที
```
