# Re-render Anti-patterns

## ❌ Mistake 1: State อยู่สูงเกินไป

```tsx
function App() {
  const [searchQuery, setSearchQuery] = useState('')  // ทำให้ App re-render ทั้งต้น 💥
  return (
    <div>
      <Header />      {/* re-render โดยไม่จำเป็น */}
      <Sidebar />     {/* re-render โดยไม่จำเป็น */}
      <SearchBar query={searchQuery} onChange={setSearchQuery} />
    </div>
  )
}
```

✅ Fix: ย้าย state ลงมาใกล้ที่ใช้

```tsx
function SearchSection() {
  const [searchQuery, setSearchQuery] = useState('')
  return (
    <>
      <SearchBar query={searchQuery} onChange={setSearchQuery} />
      <Results query={searchQuery} />
    </>
  )
}
```

---

## ❌ Mistake 2: key ใช้ index

```tsx
// ❌ bug ตอน reorder/filter — React ไม่รู้ว่า item ไหนคือตัวไหน
{items.map((item, index) => (
  <ProductCard key={index} product={item} />
))}
```

✅ Fix: ใช้ unique stable ID เสมอ

```tsx
{items.map((item) => (
  <ProductCard key={item.id} product={item} />
))}
```

---

## ❌ Mistake 3: Object/function ใหม่ทุก render ส่งให้ memo component

```tsx
// ❌ MemoizedChild re-render ทุกครั้งเพราะ onSubmit เป็น function ใหม่
const MemoizedChild = memo(ChildComponent)

function Parent() {
  const handleSubmit = (data) => { ... }  // ใหม่ทุก render
  return <MemoizedChild onSubmit={handleSubmit} />
}
```

✅ Fix: useCallback

```tsx
const handleSubmit = useCallback((data) => { ... }, [/* deps */])
```

---

## ❌ Mistake 4: Context re-render ทั้ง tree

```tsx
// ❌ value object สร้างใหม่ทุก render → ทุก consumer re-render
<AuthContext.Provider value={{ user, login, logout }}>
```

✅ Fix: useMemo ครอบ value

```tsx
const value = useMemo(() => ({ user, login, logout }), [user, login, logout])
<AuthContext.Provider value={value}>
```
