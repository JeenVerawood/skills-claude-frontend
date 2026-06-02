# Middleware มาตรฐาน

## Pattern ที่ถูกต้อง

```ts
// middleware.ts (root level)
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(req: NextRequest) {
  const token = req.cookies.get('auth-token')?.value
  const { pathname } = req.nextUrl

  // Protected routes
  if (pathname.startsWith('/dashboard') && !token) {
    return NextResponse.redirect(new URL('/login', req.url))
  }

  // Auth routes (redirect ถ้า login แล้ว)
  if (pathname.startsWith('/login') && token) {
    return NextResponse.redirect(new URL('/dashboard', req.url))
  }

  return NextResponse.next()
}

// ระบุ matcher เฉพาะ path ที่ต้องการ
export const config = {
  matcher: [
    '/dashboard/:path*',
    '/admin/:path*',
    '/login',
    '/register',
    '/((?!_next/static|_next/image|favicon.ico).*)',
  ],
}
```

## กฎ Middleware

- ห้าม matcher `'/:path*'` (ครอบทั้งหมด) → ช้า
- ต้องยกเว้น `_next/static`, `_next/image`, `favicon.ico`
- Logic หนักๆ ควรทำใน API route ไม่ใช่ middleware
