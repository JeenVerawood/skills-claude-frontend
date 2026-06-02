# next/image & next/font

## next/image — การใช้งานถูกต้อง

```tsx
import Image from 'next/image'

// Hero — priority โหลดก่อน (ส่งผล LCP)
<Image src="/hero.jpg" alt="Hero" width={1920} height={1080} priority quality={85} />

// Fill container
<div className="relative h-64 w-full">
  <Image src="/cover.jpg" alt="Cover" fill style={{ objectFit: 'cover' }}
    sizes="(max-width: 768px) 100vw, 50vw" />
</div>

// Gallery — lazy load (default)
<Image src={img.url} alt={img.alt} width={400} height={300}
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw" />
```

## next.config.js — remote images

```js
const nextConfig = {
  images: {
    remotePatterns: [
      { protocol: 'https', hostname: 'your-cdn.com', pathname: '/images/**' },
      { protocol: 'https', hostname: '**.cloudfront.net' },
    ],
  },
}
```

## Mockup / Placeholder Images — กฎบังคับ

**ห้ามใช้ Unsplash direct photo ID** — URL รูปแบบ `images.unsplash.com/photo-XXXXXXXXX` จะ 404 เมื่อรูปถูกลบ/เปลี่ยน

### บริการที่อนุญาต

| บริการ | URL Pattern | เหมาะกับ |
|---|---|---|
| placehold.co | `https://placehold.co/400x300` | generic placeholder สีเทา |
| picsum.photos (seed) | `https://picsum.photos/seed/{keyword}/400/300` | photorealistic ตรงกับ domain |

### กฎเลือก keyword

- **ต้องอ่าน spec domain ก่อน** แล้วเลือก seed ให้ตรงกับ project
- ❌ `seed/photo1`, `seed/image`, `seed/placeholder` — ไม่สื่อ domain
- ✅ `seed/dog` สำหรับ pet shop, `seed/food` สำหรับร้านอาหาร, `seed/house` สำหรับ real estate

### remotePatterns สำหรับ mockup

```js
images: {
  remotePatterns: [
    { protocol: 'https', hostname: 'picsum.photos' },
    { protocol: 'https', hostname: 'placehold.co' },
  ],
}
```

## next/font — ไม่มี layout shift

```tsx
// app/layout.tsx
import { Inter, Sarabun } from 'next/font/google'

const inter = Inter({ subsets: ['latin'], variable: '--font-inter', display: 'swap' })
const sarabun = Sarabun({
  subsets: ['thai', 'latin'],
  weight: ['400', '500', '700'],
  variable: '--font-sarabun',
  display: 'swap',
})

export default function RootLayout({ children }) {
  return (
    <html className={`${inter.variable} ${sarabun.variable}`}>
      <body>{children}</body>
    </html>
  )
}
```
