# Hydration Mismatch — Anti-patterns

## ❌ Mistake 1: ค่า random/date ต่างกันระหว่าง server/client

```tsx
// ❌ server render ได้ค่าหนึ่ง, client render ได้อีกค่า
const id = Math.random()
const now = new Date().toString()
```

✅ Fix: `useId()` สำหรับ ID, `suppressHydrationWarning` สำหรับ timestamp

```tsx
import { useId } from 'react'
const id = useId()  // consistent ระหว่าง server/client

<time suppressHydrationWarning>{new Date().toLocaleString()}</time>
```

---

## ❌ Mistake 2: Browser API ใน render scope

```tsx
// ❌ server ไม่มี localStorage, window
const theme = localStorage.getItem('theme')
const width = window.innerWidth
```

✅ Fix: ย้ายเข้า useEffect

```tsx
const [theme, setTheme] = useState('light')
useEffect(() => {
  setTheme(localStorage.getItem('theme') || 'light')
}, [])
```

---

## ❌ Mistake 3: Component ที่ใช้ browser API ไม่ได้ปิด SSR

```tsx
// ❌ import component ที่ใช้ window ตรงๆ
import { MapComponent } from './Map'  // Map ใช้ window.google
```

✅ Fix: dynamic import พร้อม ssr: false

```tsx
import dynamic from 'next/dynamic'
const Map = dynamic(() => import('./Map'), { ssr: false })
```

---

## ❌ Mistake 4: Conditional render ต่างกันระหว่าง server/client

```tsx
// ❌ server render ไม่มี user, client render มี → mismatch
export default function Nav() {
  const user = getCurrentUser()  // client-side only
  return user ? <UserNav /> : <GuestNav />
}
```

✅ Fix: ใช้ useEffect + loading state

```tsx
const [user, setUser] = useState<User | null>(null)
const [mounted, setMounted] = useState(false)

useEffect(() => {
  setUser(getCurrentUser())
  setMounted(true)
}, [])

if (!mounted) return <NavSkeleton />
return user ? <UserNav /> : <GuestNav />
```
