# Zod Patterns

## ทำไมต้องใช้ Zod

```
TypeScript = compile-time safety เท่านั้น
Zod        = runtime validation → ป้องกัน API response ที่ไม่ตรง shape
```

## Patterns หลัก

```ts
import { z } from 'zod'

// parse — throw ZodError ถ้าไม่ตรง
const user = UserSchema.parse(data)

// safeParse — ไม่ throw, return { success, data/error }
const result = UserSchema.safeParse(data)
if (result.success) {
  console.log(result.data)
} else {
  console.error(result.error.issues)
}

// coerce — แปลง type อัตโนมัติ
z.coerce.number()   // "42" → 42
z.coerce.date()     // "2024-01-01" → Date

// transform — แปลงค่า
z.string().transform(s => s.trim().toLowerCase())

// default — ค่า default ถ้า undefined
z.string().default('user')
z.number().default(1)

// optional vs nullable
z.string().optional()   // string | undefined
z.string().nullable()   // string | null
z.string().nullish()    // string | null | undefined
```

## Schema Composition

```ts
const BaseSchema = z.object({ id: z.string(), createdAt: z.coerce.date() })

const UserSchema = BaseSchema.extend({
  name: z.string().min(1).max(100),
  email: z.string().email(),
})

// Derive schemas
const CreateSchema = UserSchema.omit({ id: true, createdAt: true })
const UpdateSchema = CreateSchema.partial()
const PreviewSchema = UserSchema.pick({ id: true, name: true })

// Type inference
type User        = z.infer<typeof UserSchema>
type CreateInput = z.infer<typeof CreateSchema>
```

→ ดู templates สำเร็จรูปใน `../../05-ts-zod/templates/zod-schemas.md`
