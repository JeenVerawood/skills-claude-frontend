# Template: Error & Not-Found Files

## error.tsx (copy ได้เลย)

```tsx
'use client'
import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    console.error(error)
    // reportToSentry(error)
  }, [error])

  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] gap-4">
      <h2 className="text-xl font-semibold">เกิดข้อผิดพลาด</h2>
      <p className="text-sm text-gray-500">{error.message}</p>
      {error.digest && (
        <p className="text-xs text-gray-400">Error ID: {error.digest}</p>
      )}
      <button
        onClick={reset}
        className="px-4 py-2 bg-black text-white rounded"
      >
        ลองใหม่อีกครั้ง
      </button>
    </div>
  )
}
```

## not-found.tsx (copy ได้เลย)

```tsx
import Link from 'next/link'

export default function NotFound() {
  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] gap-4">
      <h2 className="text-xl font-semibold">ไม่พบหน้าที่ต้องการ</h2>
      <p className="text-sm text-gray-500">หน้านี้ไม่มีอยู่หรือถูกลบไปแล้ว</p>
      <Link href="/" className="px-4 py-2 bg-black text-white rounded">
        กลับหน้าแรก
      </Link>
    </div>
  )
}
```

## loading.tsx (copy ได้เลย)

```tsx
export default function Loading() {
  return (
    <div className="flex items-center justify-center min-h-[400px]">
      <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-gray-900" />
    </div>
  )
}
```
