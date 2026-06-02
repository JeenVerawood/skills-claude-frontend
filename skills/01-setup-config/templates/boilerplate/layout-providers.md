# Layout + Providers มาตรฐาน (copy ได้เลย)

## src/app/providers.tsx

```tsx
'use client'

import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { useState } from 'react'

export function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 1000 * 60,       // 1 minute
            retry: 2,
            refetchOnWindowFocus: false,
          },
          mutations: {
            retry: 0,
          },
        },
      })
  )

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      {process.env.NODE_ENV === 'development' && (
        <ReactQueryDevtools initialIsOpen={false} />
      )}
    </QueryClientProvider>
  )
}
```

---

## src/app/layout.tsx

```tsx
import type { Metadata } from 'next'
import { Sarabun } from 'next/font/google'
import { Providers } from './providers'
import './globals.css'

const sarabun = Sarabun({
  subsets: ['thai', 'latin'],
  weight: ['400', '500', '600', '700'],
  variable: '--font-sans',
  display: 'swap',
})

export const metadata: Metadata = {
  title: {
    default: process.env.NEXT_PUBLIC_APP_NAME ?? 'My App',
    template: `%s | ${process.env.NEXT_PUBLIC_APP_NAME ?? 'My App'}`,
  },
  description: 'My App description',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="th" className={sarabun.variable}>
      <body className="min-h-screen bg-white font-sans antialiased">
        <Providers>{children}</Providers>
      </body>
    </html>
  )
}
```

## กฎสำคัญ

```
layout.tsx → Server Component (ไม่มี 'use client')
providers.tsx → Client Component ('use client' อยู่ที่นี่)

ห้ามใส่ useState / useEffect ใน layout.tsx โดยตรง
→ ให้ย้ายเข้า providers.tsx แทน
```
