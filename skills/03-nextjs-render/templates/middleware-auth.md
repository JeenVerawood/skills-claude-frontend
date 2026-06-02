# Template: Middleware Auth (copy ได้เลย)

```ts
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

const PROTECTED = ['/dashboard', '/admin', '/settings', '/profile']
const ADMIN_ONLY = ['/admin']
const AUTH_ONLY  = ['/login', '/register']

export async function middleware(req: NextRequest) {
  const { pathname } = req.nextUrl
  const token = req.cookies.get('auth-token')?.value
  const role  = req.cookies.get('user-role')?.value

  const isProtected = PROTECTED.some((p) => pathname.startsWith(p))
  const isAdminOnly = ADMIN_ONLY.some((p) => pathname.startsWith(p))
  const isAuthOnly  = AUTH_ONLY.some((p) => pathname.startsWith(p))

  if (isAuthOnly && token) {
    return NextResponse.redirect(new URL('/dashboard', req.url))
  }

  if (isProtected && !token) {
    const url = new URL('/login', req.url)
    url.searchParams.set('callbackUrl', pathname)
    return NextResponse.redirect(url)
  }

  if (isAdminOnly && role !== 'admin') {
    return NextResponse.redirect(new URL('/403', req.url))
  }

  return NextResponse.next()
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico|public/).*)'],
}
```
