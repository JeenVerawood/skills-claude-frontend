# next.config.js มาตรฐาน (copy ได้เลย)

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      { protocol: 'https', hostname: '**.amazonaws.com' },
      { protocol: 'https', hostname: '**.cloudfront.net' },
    ],
  },

  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          { key: 'X-Frame-Options',        value: 'SAMEORIGIN' },
          { key: 'X-Content-Type-Options',  value: 'nosniff' },
          { key: 'Referrer-Policy',         value: 'strict-origin-when-cross-origin' },
          { key: 'Permissions-Policy',      value: 'camera=(), microphone=(), geolocation=()' },
        ],
      },
    ]
  },

  // output: 'standalone', // เปิดเมื่อ deploy ด้วย Docker
}

module.exports = nextConfig
```

## เมื่อไหรต้องแก้

```
เพิ่ม remote image domain  → เพิ่มใน remotePatterns
Deploy ด้วย Docker         → เปิด output: 'standalone'
ต้องการ redirect           → เพิ่ม async redirects()
```
