# State Management Anti-patterns

## ❌ Mistake 1: Server data ใน global state

```ts
// ❌ Zustand เก็บ server data + sync เอง = stale data nightmare
const useStore = create((set) => ({
  users: [],
  fetchUsers: async () => {
    const data = await fetch('/api/users').then(r => r.json())
    set({ users: data })
  },
}))

useEffect(() => { fetchUsers() }, [])  // ❌ manual sync
```

✅ Fix: TanStack Query จัดการ cache + revalidation ให้

```ts
const { data, isLoading } = useQuery({
  queryKey: ['users'],
  queryFn: () => fetch('/api/users').then(r => r.json()),
})
```

---

## ❌ Mistake 2: Subscribe ทั้ง store

```ts
// ❌ re-render ทุกครั้งที่ state ส่วนไหนเปลี่ยน
const { items, isLoading, theme, modal } = useCartStore()
```

✅ Fix: Selector เฉพาะที่ต้องการ

```ts
const items     = useCartStore((s) => s.items)
const isLoading = useCartStore((s) => s.isLoading)
```

---

## ❌ Mistake 3: Redux สำหรับ global UI state เล็กๆ

```ts
// ❌ over-engineering — Redux สำหรับแค่ modal open/close
const modalSlice = createSlice({ ... })
// actions, reducers, dispatch, connect... ทั้งหมดนี้เพื่อ boolean เดียว!
```

✅ Fix: Zustand ลดเหลือแค่นี้

```ts
const useUiStore = create<{ isOpen: boolean; toggle: () => void }>((set) => ({
  isOpen: false,
  toggle: () => set((s) => ({ isOpen: !s.isOpen })),
}))
```

---

## ❌ Mistake 4: Filter/Pagination ใน global state

```ts
// ❌ ปิดหน้า → filter หาย, ไม่สามารถ share URL ได้
const useFilterStore = create(() => ({ page: 1, search: '', role: 'all' }))
```

✅ Fix: URL state ด้วย useSearchParams

```ts
const searchParams = useSearchParams()
const page = Number(searchParams.get('page') ?? '1')
// → shareable URL, bookmarkable, browser back ทำงาน
```
