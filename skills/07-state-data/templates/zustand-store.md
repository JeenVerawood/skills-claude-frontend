# Template: Zustand Store สำเร็จรูป

## Cart Store (copy ได้เลย)

```ts
// src/stores/cart.store.ts
import { create } from 'zustand'
import { devtools, persist } from 'zustand/middleware'

type CartItem = { productId: string; name: string; price: number; quantity: number }

type CartStore = {
  items:      CartItem[]
  isLoading:  boolean
  addItem:    (item: Omit<CartItem, 'quantity'>) => void
  removeItem: (productId: string) => void
  updateQty:  (productId: string, quantity: number) => void
  clearCart:  () => void
  total:      () => number
}

export const useCartStore = create<CartStore>()(
  devtools(
    persist(
      (set, get) => ({
        items:     [],
        isLoading: false,

        addItem: (item) => set((s) => {
          const existing = s.items.find((i) => i.productId === item.productId)
          return existing
            ? { items: s.items.map((i) => i.productId === item.productId
                ? { ...i, quantity: i.quantity + 1 } : i) }
            : { items: [...s.items, { ...item, quantity: 1 }] }
        }),

        removeItem: (productId) =>
          set((s) => ({ items: s.items.filter((i) => i.productId !== productId) })),

        updateQty: (productId, quantity) =>
          set((s) => ({ items: s.items.map((i) =>
            i.productId === productId ? { ...i, quantity } : i) })),

        clearCart: () => set({ items: [] }),
        total: () => get().items.reduce((sum, i) => sum + i.price * i.quantity, 0),
      }),
      { name: 'cart-storage', partialize: (s) => ({ items: s.items }) }
    ),
    { name: 'CartStore' }
  )
)
```
