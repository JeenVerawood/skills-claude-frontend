# useEffect Anti-patterns

## ❌ Mistake 1: Infinite Loop — state update ใน effect ที่ depend on state เดิม

```tsx
useEffect(() => {
  setData(process(data))  // data เปลี่ยน → effect รัน → loop 💥
}, [data])
```

✅ Fix: ใช้ useMemo แทน

```tsx
const processedData = useMemo(() => process(data), [data])
```

---

## ❌ Mistake 2: Infinite Loop — Object/Array dependency สร้างใหม่ทุก render

```tsx
useEffect(() => {
  fetchData(options)
}, [options])  // options = {} reference ใหม่ทุก render 💥
```

✅ Fix: useMemo ครอบ object dependency

```tsx
const options = useMemo(() => ({ limit: 10, page: 1 }), [])
useEffect(() => {
  fetchData(options)
}, [options])
```

---

## ❌ Mistake 3: Stale Closure

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log(count)  // ได้ 0 ตลอด — count ค้างตอน mount 💥
  }, 1000)
  return () => clearInterval(interval)
}, [])  // ลืมใส่ count
```

✅ Fix: ใส่ dependency ครบ หรือใช้ useRef

```tsx
// Option 1: ใส่ dependency
useEffect(() => { ... }, [count])

// Option 2: useRef เก็บค่าล่าสุด
const countRef = useRef(count)
useEffect(() => { countRef.current = count }, [count])
```

---

## ❌ Mistake 4: Async ไม่มี cleanup → race condition

```tsx
useEffect(() => {
  fetchUser(userId).then(setUser)  // ถ้า userId เปลี่ยนเร็ว response เก่ามาทีหลัง 💥
}, [userId])
```

✅ Fix: ดูไฟล์ `templates/async-effect.md`

---

## ❌ Mistake 5: ลืม dependency → ESLint warning ที่ถูกต้อง

```tsx
// ❌ suppress warning แทนที่จะแก้
// eslint-disable-next-line react-hooks/exhaustive-deps
useEffect(() => {
  doSomething(value)
}, [])
```

✅ Fix: ใส่ dependency จริงๆ หรือใช้ useCallback ครอบ function

---

## ❌ Mistake 6: eslint-disable-line แทนที่จะแก้ root cause

// อาการ: เห็น eslint-disable-line react-hooks/exhaustive-deps
// ความหมาย: มี stale closure ซ่อนอยู่ที่ยังไม่ได้แก้

```ts
useEffect(() => {
  toggleSpecies(species)
}, [searchParams]) // eslint-disable-line ← 🚩 red flag
```

// root cause เกือบทุกครั้งคือ function ใน deps ไม่มี stable reference
// วิธีแก้ที่ถูก: ทำให้ function นั้น stable ก่อน (useCallback)
// แล้วค่อยใส่ใน deps จริงๆ
