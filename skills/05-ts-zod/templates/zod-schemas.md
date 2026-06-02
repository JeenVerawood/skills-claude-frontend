# Template: Zod Schemas (copy ได้เลย)

## User Schema

```ts
import { z } from 'zod'

export const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1).max(100),
  email: z.string().email(),
  role: z.enum(['admin', 'user', 'guest']),
  createdAt: z.coerce.date(),
})

export type User = z.infer<typeof UserSchema>

// Derived schemas
export const CreateUserSchema = UserSchema.omit({ id: true, createdAt: true })
export const UpdateUserSchema = UserSchema.omit({ id: true, createdAt: true }).partial()
export const UserPreviewSchema = UserSchema.pick({ id: true, name: true })

export type CreateUserInput = z.infer<typeof CreateUserSchema>
export type UpdateUserInput = z.infer<typeof UpdateUserSchema>
export type UserPreview     = z.infer<typeof UserPreviewSchema>
```

## API Response Schema

```ts
export const ApiSuccessSchema = <T extends z.ZodTypeAny>(dataSchema: T) =>
  z.object({
    data: dataSchema,
    message: z.string().optional(),
  })

export const PaginatedSchema = <T extends z.ZodTypeAny>(itemSchema: T) =>
  z.object({
    items: z.array(itemSchema),
    total: z.number(),
    page: z.number(),
    pageSize: z.number(),
    hasNext: z.boolean(),
  })

// ใช้งาน
const UsersResponseSchema = PaginatedSchema(UserSchema)
type UsersResponse = z.infer<typeof UsersResponseSchema>
```

## Environment Schema

```ts
export const EnvSchema = z.object({
  NODE_ENV: z.enum(['development', 'test', 'production']),
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  NEXT_PUBLIC_API_URL: z.string().url(),
})

export const env = EnvSchema.parse(process.env)
// crash ตั้งแต่ start server ถ้า env ไม่ครบ/ไม่ถูกต้อง
```

## Form Schema (React Hook Form + Zod)

```ts
export const LoginSchema = z.object({
  email: z.string().email('กรุณากรอก email ที่ถูกต้อง'),
  password: z.string().min(8, 'รหัสผ่านต้องมีอย่างน้อย 8 ตัวอักษร'),
})

export type LoginInput = z.infer<typeof LoginSchema>
```
