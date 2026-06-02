# Template: next/image Patterns

```tsx
import Image from 'next/image'

// Hero — above fold, priority load (ส่งผล LCP มากที่สุด)
<Image
  src="/hero.jpg"
  alt="Hero banner"
  width={1920}
  height={1080}
  priority
  quality={85}
/>

// Fill container — รูปที่ต้อง cover พื้นที่
<div className="relative h-64 w-full overflow-hidden rounded-lg">
  <Image
    src="/cover.jpg"
    alt="Cover"
    fill
    style={{ objectFit: 'cover' }}
    sizes="(max-width: 768px) 100vw, 50vw"
  />
</div>

// Avatar — รูปกลม
<div className="relative h-10 w-10">
  <Image
    src={user.avatarUrl}
    alt={user.name}
    fill
    className="rounded-full object-cover"
    sizes="40px"
  />
</div>

// Product gallery — responsive grid
{products.map((p) => (
  <div key={p.id} className="relative aspect-square">
    <Image
      src={p.imageUrl}
      alt={p.name}
      fill
      className="object-contain"
      sizes="(max-width: 640px) 50vw, (max-width: 1024px) 33vw, 25vw"
    />
  </div>
))}

// Blur placeholder
<Image
  src="/photo.jpg"
  alt="Photo"
  width={800}
  height={600}
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRg..."
/>
```
