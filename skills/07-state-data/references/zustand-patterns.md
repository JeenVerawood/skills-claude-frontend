# Zustand Patterns

## Store มาตรฐาน

```ts
// src/stores/ui.store.ts
import { create } from 'zustand'
import { devtools } from 'zustand/middleware'

type UiStore = {
  activeModal: string | null
  sidebarOpen: boolean
  openModal:    (name: string) => void
  closeModal:   () => void
  toggleSidebar: () => void
}

export const useUiStore = create<UiStore>()(
  devtools(
    (set) => ({
      activeModal:  null,
      sidebarOpen:  true,
      openModal:    (name) => set({ activeModal: name }),
      closeModal:   () => set({ activeModal: null }),
      toggleSidebar: () => set((s) => ({ sidebarOpen: !s.sidebarOpen })),
    }),
    { name: 'UiStore' }
  )
)
```

## Selector Pattern — ป้องกัน re-render

```ts
// ❌ subscribe ทั้ง store
const { items, theme, modal } = useCartStore()

// ✅ เลือกเฉพาะที่ต้องการ
const items   = useCartStore((s) => s.items)
const total   = useCartStore((s) => s.items.reduce((sum, i) => sum + i.price, 0))
const addItem = useCartStore((s) => s.addItem)  // actions ไม่เปลี่ยน reference
```

## ⚠️ Type Parameter Naming — ป้องกัน ESLint warn

```ts
// ❌ ตั้งชื่อ parameter ใน type declaration เหมือน field ใน store
type AuthStore = {
  user: User | null
  setUser: (user: User | null) => void   // ← ESLint warn: 'user' is defined but never used
}

// เหตุผล: ใน type-only function signature, parameter name ถือว่า "ไม่ได้ใช้งาน"
//         ESLint no-unused-vars จับได้แม้จะเป็นแค่ชื่อ type parameter
```

✅ Fix: ตั้งชื่อ parameter ใน type store ให้ขึ้นต้น `_` เสมอ

```ts
// ✅ ขึ้นต้น _ → ตรง argsIgnorePattern/varsIgnorePattern → ไม่ warn
type AuthStore = {
  user: User | null
  setUser: (_value: User | null) => void  // ← ชัดเจน: parameter นี้เป็น type-only
  clear: () => void
}

export const useAuthStore = create<AuthStore>()(
  persist(
    (set) => ({
      user:    null,
      setUser: (value) => set({ user: value }),  // ← ตัว implement ใช้ชื่อปกติได้
      clear:   () => set({ user: null }),
    }),
    { name: 'auth-storage', partialize: (s) => ({ user: s.user }) }
  )
)
```

กฎ: **type declaration ของ store method → ใช้ `_` prefix เสมอ**
    **ส่วน implementation (ใน `create()`) → ตั้งชื่อตามปกติได้**

---

## Persist Store

```ts
import { persist } from 'zustand/middleware'

export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({ items: [], addItem: (item) => set((s) => ({ items: [...s.items, item] })) }),
    {
      name: 'cart-storage',
      partialize: (state) => ({ items: state.items }),  // เก็บแค่ items ใน localStorage
    }
  )
)
```
