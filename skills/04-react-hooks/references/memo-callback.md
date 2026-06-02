# useMemo & useCallback

## เมื่อไหรใช้

```
useMemo    → cache ผลลัพธ์ expensive calculation
useCallback → cache function reference (ป้องกัน memo child re-render)

⚠️ ไม่ใช่ทุก case ต้องใช้ — มีต้นทุน memory + complexity
```

## useMemo

```tsx
// ✅ calculation หนัก
const sortedItems = useMemo(
  () => [...items].sort((a, b) => b.price - a.price),
  [items]
)

// ✅ object/array dependency สำหรับ useEffect
const queryParams = useMemo(
  () => ({ page, limit, search }),
  [page, limit, search]
)

// ❌ ไม่จำเป็น — calculation เบามาก
const double = useMemo(() => count * 2, [count])
// แค่นี้พอ: const double = count * 2
```

## useCallback

```tsx
// ✅ function ที่ส่งให้ memo child
const handleSubmit = useCallback(async (data: FormData) => {
  await submitForm(data)
  onSuccess()
}, [onSuccess])

// ✅ function ที่ใช้ใน useEffect dependency
const fetchData = useCallback(() => {
  return fetch(`/api/data?page=${page}`)
}, [page])

useEffect(() => { fetchData() }, [fetchData])

// ❌ ไม่จำเป็น — ถ้าไม่ได้ส่งให้ memo child
const handleClick = useCallback(() => {
  setCount(c => c + 1)
}, [])   // ใช้แค่ () => setCount(c => c + 1) ตรงๆ ก็พอ
```

## Quick Decision

```
ต้องส่ง function ให้ memo component?  → useCallback
ต้องใช้ function ใน useEffect?        → useCallback
calculation หนัก (>1ms)?              → useMemo
object/array ใน useEffect dependency? → useMemo
อื่นๆ                                 → ไม่ต้อง memo
```

---

## Custom Hook Pattern — ต้อง useCallback ทุก function ที่ export ออกไป

เมื่อ custom hook return functions ออกไปให้ component อื่นใช้
ทุก function ต้อง wrap ด้วย useCallback เสมอ
เพราะ component ที่รับไปอาจ:
1. ใส่ function นั้นใน useEffect dependency
2. ส่งต่อให้ memo component

❌ ปัญหา
```ts
const useFilters = () => {
  const toggleSpecies = (s: string) => { ... }  // ใหม่ทุก render
  return { toggleSpecies }
}

// component อื่น
useEffect(() => {
  toggleSpecies(species)
}, [searchParams, toggleSpecies])  // toggleSpecies เปลี่ยนทุก render → effect fire ตลอด
```

✅ แก้
```ts
const useFilters = () => {
  const toggleSpecies = useCallback((s: string) => {
    setFilters(prev => ...)  // functional update → ไม่ต้องใส่ filters ใน deps
  }, [])  // stable reference
  return { toggleSpecies }
}
```
