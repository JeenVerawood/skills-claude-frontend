# Image & Font Optimization

## next/image — กฎ

```
ใช้ <Image /> แทน <img> เสมอ
Hero image ใส่ priority
ใส่ sizes attribute สำหรับ responsive images
remote images ต้อง config remotePatterns ใน next.config.js
```

## next/font — กฎ

```
ใช้ next/font แทน Google Fonts <link> เสมอ
→ ไม่มี extra network request
→ ไม่มี layout shift (CLS = 0)
→ font file อยู่ใน bundle
```

→ code examples: `../../03-nextjs-render/references/next-image-font.md`
→ image patterns: `templates/image-patterns.md`
