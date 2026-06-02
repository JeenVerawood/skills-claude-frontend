# useState vs useRef

## เลือกใช้แบบไหน

```
useState → ต้องการให้ component re-render เมื่อค่าเปลี่ยน
useRef   → เก็บค่าโดยไม่ trigger re-render / เข้าถึง DOM element
```

## ตัวอย่าง

```tsx
// ✅ useState: ค่าที่แสดงใน UI
const [count, setCount]   = useState(0)
const [isOpen, setIsOpen] = useState(false)
const [user, setUser]     = useState<User | null>(null)

// ✅ useRef: ค่าที่ไม่ต้องแสดงผล
const timerRef    = useRef<ReturnType<typeof setInterval> | null>(null)
const prevValue   = useRef(value)       // เก็บ previous value
const inputRef    = useRef<HTMLInputElement>(null)   // DOM access
const isMounted   = useRef(false)       // track mount state

// ❌ useRef เก็บค่าที่ต้องแสดง UI → ไม่ re-render!
const count = useRef(0)
count.current++   // UI จะไม่เปลี่ยน!
```

## Pattern: เก็บ previous value

```tsx
function Component({ value }: { value: number }) {
  const prevRef = useRef(value)

  useEffect(() => {
    prevRef.current = value
  })

  return <div>Current: {value}, Previous: {prevRef.current}</div>
}
```
