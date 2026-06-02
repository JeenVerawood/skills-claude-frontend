# src/lib/utils.ts (copy ได้เลย)

```ts
import { type ClassValue, clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

// ───────────────────────────────────────────
// Tailwind class merger
// ใช้แทน className string concat เสมอ
// ───────────────────────────────────────────
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

// ───────────────────────────────────────────
// Format currency
// ───────────────────────────────────────────
export function formatCurrency(
  amount: number,
  currency = 'THB',
  locale = 'th-TH'
): string {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency,
    minimumFractionDigits: 0,
  }).format(amount)
}

// ───────────────────────────────────────────
// Format date
// ───────────────────────────────────────────
export function formatDate(
  date: Date | string,
  options: Intl.DateTimeFormatOptions = {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  }
): string {
  return new Intl.DateTimeFormat('th-TH', options).format(new Date(date))
}

// ───────────────────────────────────────────
// Truncate text
// ───────────────────────────────────────────
export function truncate(str: string, length: number): string {
  return str.length > length ? `${str.slice(0, length)}...` : str
}

// ───────────────────────────────────────────
// Sleep — dev/test เท่านั้น
// ───────────────────────────────────────────
export const sleep = (ms: number) =>
  new Promise((resolve) => setTimeout(resolve, ms))
```
