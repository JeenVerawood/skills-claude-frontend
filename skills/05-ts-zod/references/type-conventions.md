# Type Conventions

## interface vs type

```ts
// interface → Object shapes, extendable
interface User { id: string; name: string }
interface AdminUser extends User { permissions: string[] }

// type → Union, Intersection, Mapped, Aliases
type Role   = 'admin' | 'user' | 'guest'
type Status = 'idle' | 'loading' | 'success' | 'error'
type Result<T> = { success: true; data: T } | { success: false; error: string }
```

กฎง่ายๆ: **ต้องการ extends/implements → interface, อื่นๆ → type**

## Utility Types Cheatsheet

```ts
interface User { id: string; name: string; email: string; role: string; createdAt: Date }

type CreateInput  = Omit<User, 'id' | 'createdAt'>        // ตัด field
type UpdateInput  = Partial<Omit<User, 'id'>>              // optional ทั้งหมด
type Preview      = Pick<User, 'id' | 'name'>              // เลือก field
type RoleMap      = Record<string, string[]>               // key-value
type StrictUser   = Required<User>                         // required ทั้งหมด

// Function types
type GetUser      = typeof fetchUser                        // function type
type GetReturn    = Awaited<ReturnType<typeof fetchUser>>   // return type
type GetParams    = Parameters<typeof fetchUser>            // params type
```
