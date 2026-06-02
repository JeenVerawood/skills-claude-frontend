---
name: 08-auth-guard
description: >
  Use this skill for any authentication or security question in Next.js: JWT storage, cookie
  security attributes, NextAuth.js setup, Middleware auth protection, API route auth guards,
  CSRF protection, XSS prevention, role-based access control. Triggers include: "auth", "JWT",
  "cookie", "httpOnly", "session", "NextAuth", "middleware protect", "CSRF", "XSS", "role",
  "permission", "guard", "unauthorized", "ป้องกัน", "เช็คสิทธิ์", "login", "logout", "token".
  Always use this skill before writing any authentication logic, protected route, or security code.
---

# 08-auth-guard: Auth & Security

## Operating Stance

- **Adversarial mindset.** คิดแบบ attacker เสมอ — "ถ้าฉันเป็นคน bypass auth ฉันจะทำยังไง?" แล้วปิดช่องนั้น
- **Defense in depth.** Auth ต้องมีทุกชั้น — Middleware + API route + Server Component — ไม่ใช่ชั้นเดียว
- **Secure by default.** ถ้าไม่แน่ใจว่าควร allow หรือ deny → deny ก่อนเสมอ

---

## When NOT to use

- **ปัญหาเป็น state management ของ user object** → ใช้ Skill 06 (TanStack Query สำหรับ /api/me)
- **ปัญหาเป็น API Route handler ทั่วไป** → ใช้ Skill 05 แต่ auth check ต้องอ้าง Skill นี้
- **แค่ถาม form validation** → ใช้ Skill 06 (React Hook Form)

---

## Refuse to proceed ถ้า

```
❌ user ขอเก็บ JWT ใน localStorage → ปฏิเสธทันที อธิบาย XSS risk แล้วเสนอ httpOnly cookie
❌ user ขอ auth check เฉพาะ client-side → อธิบาย security hole แล้วบังคับ server-side check ด้วย
❌ user ขอ disable security header โดยไม่มีเหตุผล → ถามเหตุผล อธิบาย risk ก่อน comply
❌ ไม่รู้ว่า route ต้องการ role ไหน → ถามก่อน ห้าม assume ว่า "user ธรรมดา" ก็พอ
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/auth-layers.md` | **อ่านก่อนเสมอ** — layered auth model ทุกชั้น |
| `references/cookie-security.md` | ถามเรื่อง cookie attributes, JWT storage |
| `references/rbac.md` | ถามเรื่อง role, permission, access control |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/auth-mistakes.md` | JWT ใน localStorage, auth แค่ client, cookie ไม่ปลอดภัย |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/auth-helpers.md` | getSession, requireAuth, requireRole สำเร็จรูป |
| `templates/login-route.md` | Login/Logout API route สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. **อธิบาย threat model** — attack vector ที่กำลังป้องกันคืออะไร
2. แสดง ❌ security risk + ✅ fix ครบทุกชั้น (Middleware → API Route → Component)
3. **Security Checklist** ท้ายคำตอบเสมอ

---

## Auth Layers (ต้องครบทุกชั้น)

```
ชั้นที่ 1 — Middleware (src/middleware.ts)
  → Block request ก่อนถึง page/API
  → เร็วที่สุด แต่อย่าเชื่อ 100% เพราะ bypass ได้ถ้า config ผิด

ชั้นที่ 2 — API Route (src/app/api/*/route.ts)
  → getSession() ต้องอยู่บรรทัดแรกก่อน logic ทุกอย่าง
  → ป้องกัน direct API call ที่ข้าม Middleware

ชั้นที่ 3 — Server Component / Page (src/app/*/page.tsx)
  → redirect() ถ้าไม่มี session
  → ป้องกัน SSR data leak
```

---

## Worked Example

**คำถาม:** "จะป้องกัน /dashboard ไม่ให้คนที่ไม่ได้ login เข้าได้"

**วิธีตอบที่ถูกต้อง:**
```
Threat: user พิมพ์ URL โดยตรง / bypass middleware

ต้องป้องกันทุกชั้น:

ชั้น 1 — middleware.ts (กัน request ก่อนถึง page):
export function middleware(req: NextRequest) {
  const token = req.cookies.get('session')?.value
  if (!token && req.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', req.url))
  }
}
export const config = { matcher: ['/dashboard/:path*'] }

ชั้น 2 — page.tsx (กัน SSR data leak):
export default async function DashboardPage() {
  const session = await getSession()
  if (!session) redirect('/login') // ← ต้องมีแม้ middleware จะกันแล้ว
  // ... render
}

❌ ผิด — auth แค่ client-side:
'use client'
if (!user) router.push('/login') // ← bypass ได้ด้วย JS disabled
```

**Security Checklist:**
```
□ httpOnly cookie — JWT ไม่อยู่ใน localStorage
□ Middleware ครอบทุก protected route
□ API Route มี getSession() ทุกตัว
□ Server Component redirect() ก่อน render
□ CSRF token สำหรับ mutation endpoints
```

---

## Behavioral Rules

```
[DENY-FIRST]       ถ้าไม่แน่ใจ deny ก่อนเสมอ
[ALL-LAYERS]       ห้าม auth เฉพาะชั้นเดียว — ต้องครบ Middleware + API + Component
[NO-LOCALSTORAGE]  ห้าม recommend JWT ใน localStorage ทุกกรณี
[THREAT-FIRST]     ระบุ threat ก่อนเสนอ solution เสมอ
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill REQ → spec "User Roles & Permissions" กำหนด auth requirements
ถัดไป:    Skill 05 (API) → auth helpers จาก skill นี้ใช้ใน Route Handlers
          start-all-project Phase 4 → implement auth layer ตาม spec
```
