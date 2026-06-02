# Role-Based Access Control (RBAC)

## Permission Matrix

```ts
// src/lib/permissions.ts
type Permission =
  | 'users:read' | 'users:create' | 'users:update' | 'users:delete'
  | 'admin:access' | 'reports:read'

const ROLE_PERMISSIONS: Record<string, Permission[]> = {
  admin: ['users:read', 'users:create', 'users:update', 'users:delete', 'admin:access', 'reports:read'],
  user:  ['users:read'],
  guest: [],
}

export function hasPermission(role: string, permission: Permission): boolean {
  return ROLE_PERMISSIONS[role]?.includes(permission) ?? false
}
```

## ใช้งานใน API route (proxy ไป external backend)

```ts
export async function DELETE(req: Request, { params }: Params) {
  const session = await getSession()
  if (!session) return unauthorized()
  if (!hasPermission(session.role, 'users:delete')) return forbidden()

  // proxy ไป external backend — ไม่เรียก db โดยตรง
  const res = await fetch(`${process.env.API_BASE_URL}/users/${params.id}`, {
    method: 'DELETE',
    headers: { Authorization: `Bearer ${session.accessToken}` },
  })

  if (!res.ok) {
    const err = await res.json().catch(() => ({}))
    return NextResponse.json(err, { status: res.status })
  }

  return NextResponse.json({ success: true })
}
```

## ใช้งานใน Server Component

```tsx
export default async function AdminPage() {
  const session = await getSession()
  if (!session || !hasPermission(session.role, 'admin:access')) {
    redirect('/403')
  }
  return <AdminDashboard />
}
```
