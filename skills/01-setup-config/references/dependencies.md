# Dependencies มาตรฐาน

## สร้าง Next.js project

```bash
npx create-next-app@latest my-app \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --src-dir \
  --import-alias "@/*"

cd my-app
```

## Core Dependencies (ติดตั้งทุกโปรเจกต์)

```bash
npm install \
  zod \
  clsx \
  tailwind-merge \
  @tanstack/react-query \
  @tanstack/react-query-devtools \
  zustand \
  react-hook-form \
  @hookform/resolvers
```

## Dev Dependencies

```bash
npm install -D \
  prettier \
  prettier-plugin-tailwindcss \
  @types/node
```

## Optional — เพิ่มตาม feature ที่ต้องการ

```bash
# Auth
npm install next-auth@beta

# Email
npm install resend

# Upload
npm install uploadthing
```

## หลังติดตั้งครบ — verify

```bash
npm run dev   # ต้องขึ้น http://localhost:3000 ได้
npm run build # ต้องผ่านโดยไม่มี error
npx tsc --noEmit # ต้องไม่มี type error
```
