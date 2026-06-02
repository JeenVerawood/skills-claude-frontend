# Cache Anti-patterns

## ❌ Mistake 1: fetch โดยไม่ระบุ cache strategy

```ts
// ❌ Next.js cache fetch ไว้ตลอด (default) → ข้อมูล realtime เก่า
const res = await fetch('/api/products')
```

✅ Fix: ระบุ strategy ให้ชัดเจน

```ts
const res = await fetch('/api/products', { cache: 'no-store' })         // realtime
const res = await fetch('/api/products', { next: { revalidate: 60 } })  // ISR
```

---

## ❌ Mistake 2: revalidatePath ผิด path

```ts
// ❌ path ไม่ตรงกับ route จริง → ไม่ revalidate
revalidatePath('/dashboard/users')  // แต่ route จริงคือ /dashboard/[userId]
```

✅ Fix: ระบุ path ให้ตรงกับ route segment

```ts
revalidatePath('/dashboard/[userId]', 'page')  // revalidate dynamic route
revalidatePath('/dashboard', 'layout')          // revalidate layout + children ทั้งหมด
```

---

## ❌ Mistake 3: เก็บ server data ใน useState แล้ว sync เอง

```tsx
// ❌ anti-pattern ที่เจอบ่อยมาก
const [products, setProducts] = useState([])
useEffect(() => {
  fetch('/api/products').then(r => r.json()).then(setProducts)
}, [])
// → stale data, no cache invalidation, no loading state
```

✅ Fix: ใช้ TanStack Query หรือ fetch ใน Server Component

```tsx
// Server Component
export default async function ProductPage() {
  const products = await fetchProducts()  // cache ถูกจัดการโดย Next.js
  return <ProductList products={products} />
}
```

---

## ❌ Mistake 4: ลืม tag ใน fetch → revalidateTag ไม่ทำงาน

```ts
// ❌ fetch ไม่มี tag
const res = await fetch('/api/products')

// แล้วไป revalidate tag ที่ไม่มีอยู่
revalidateTag('products')  // ไม่ทำอะไรเลย!
```

✅ Fix: ต้อง tag ตั้งแต่ตอน fetch

```ts
const res = await fetch('/api/products', { next: { tags: ['products'] } })
// แล้ว revalidateTag('products') จะทำงาน
```
