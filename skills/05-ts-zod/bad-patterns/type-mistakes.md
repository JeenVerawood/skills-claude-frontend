# TypeScript Anti-patterns

## ❌ Mistake 1: ใช้ any

```ts
// ❌ any ทำลาย type safety ทั้งระบบ
function process(data: any) {
  return data.user.name  // 💥 crash runtime ถ้า data.user ไม่มี
}

const res = await fetch('/api/user')
const user = await res.json()  // any — ไม่รู้ shape เลย
```

✅ Fix: `unknown` + Zod

```ts
const raw: unknown = await res.json()
const user = UserSchema.parse(raw)  // throw ถ้า shape ไม่ตรง
```

---

## ❌ Mistake 2: Type assertion ปิด error

```ts
// ❌ บอกให้ TypeScript เชื่อ แต่ไม่ตรวจจริง
const user = data as User
console.log(user.email.toLowerCase())  // 💥 ถ้า email เป็น undefined
```

✅ Fix: Zod validate หรือ type guard

```ts
// Zod
const user = UserSchema.parse(data)

// Type guard
function isUser(val: unknown): val is User {
  return typeof val === 'object' && val !== null && 'email' in val
}
```

---

## ❌ Mistake 3: async function ไม่ประกาศ return type

```ts
// ❌ TypeScript infer เป็น Promise<any>
const fetchUser = async (id: string) => {
  const res = await fetch(`/api/user/${id}`)
  return res.json()
}
```

✅ Fix: ดู `templates/result-pattern.md`

---

## ❌ Mistake 4: strict mode ปิด

```json
// ❌ ใน tsconfig.json
{ "compilerOptions": { "strict": false } }
// เสีย: strictNullChecks, noImplicitAny, strictFunctionTypes ทั้งหมด
```

✅ Fix: ดู `references/tsconfig-standard.md`

---

## ❌ Mistake 5: useForm ไม่มี generic type

```ts
// ❌ ไม่ระบุ generic → TypeScript infer เป็น FieldValues
// ทำให้ handleSubmit ส่ง data เป็น FieldValues แทนที่จะเป็น typed object
const form = useForm({
  resolver: zodResolver(schema),
})

// form.handleSubmit((data) => mutate(data))
// 💥 Error: Argument of type 'FieldValues' is not assignable
//           to parameter of type '{ email: string; password: string }'
```

✅ Fix: เสมอใส่ generic type ที่ match กับ Zod schema

```ts
// ✅ ระบุ generic ตรงๆ
const form = useForm<{ email: string; password: string }>({
  resolver: zodResolver(schema),
})

// หรือใช้ z.infer สำหรับ Zod schema
type FormValues = z.infer<typeof schema>

const form = useForm<FormValues>({
  resolver: zodResolver(schema),
})

// ✅ ตอนนี้ handleSubmit ส่ง data ที่ typed ถูกต้อง
form.handleSubmit((data) => mutate(data))  // data: FormValues ✓
```

กฎ: **ทุกครั้งที่เรียก `useForm()` ต้องมี generic type เสมอ** — ไม่มีข้อยกเว้น

---

## ❌ Mistake 6: Type ซ้ำซ้อนหลาย feature

```ts
// auth/types.ts
interface User { id: string; name: string }

// dashboard/types.ts
interface User { id: string; name: string; role: string }  // ❌ define ซ้ำ, inconsistent
```

✅ Fix: ทุก type อยู่ใน `src/types/` และ import จากที่เดียว
