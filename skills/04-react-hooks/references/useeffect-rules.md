# useEffect Rules

## กฎเหล็ก
1. ทุก value ที่ใช้ใน effect → ต้องอยู่ใน dependency array
2. Object/Array ใน dependency → useMemo ครอบก่อน
3. Async ใน effect → ต้องมี cleanup (ดู `templates/async-effect.md`)
4. Effect รันตลอด → เช็ค dependency ที่เปลี่ยนทุก render

## Quick Reference

```tsx
// cleanup subscription
useEffect(() => {
  const sub = subscribe(id)
  return () => sub.unsubscribe()
}, [id])

// debounce search
useEffect(() => {
  const timer = setTimeout(() => search(query), 300)
  return () => clearTimeout(timer)
}, [query])

// event listener
useEffect(() => {
  window.addEventListener('resize', handleResize)
  return () => window.removeEventListener('resize', handleResize)
}, [handleResize])
```

## Decision: ใช้ useEffect หรือไม่?

```
ต้องการ                          ใช้
────────────────────────────────────────
fetch data (server component)   → async/await ตรงใน component
fetch data (client component)   → TanStack Query / SWR
DOM manipulation                → useEffect + ref
subscription / event listener   → useEffect + cleanup
derived state จาก props/state  → useMemo (ไม่ใช่ useEffect!)
```
