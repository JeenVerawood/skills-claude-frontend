# Bundle Splitting

## Tree-shaking Imports

```ts
// ❌ import ทั้ง library
import _ from 'lodash'              // ~70kb
import * as dateFns from 'date-fns' // ~200kb

// ✅ import เฉพาะที่ใช้
import { debounce } from 'lodash-es'   // ~2kb
import { format } from 'date-fns'      // ~5kb
```

## Bundle Analysis

```bash
# ติดตั้ง
npm install @next/bundle-analyzer

# next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
})
module.exports = withBundleAnalyzer(nextConfig)

# รัน
ANALYZE=true npm run build
```

## Performance Targets

```
LCP (Largest Contentful Paint) → < 2.5s
FID (First Input Delay)        → < 100ms
CLS (Cumulative Layout Shift)  → < 0.1
First Load JS                  → < 100kb (aim for < 75kb)
```
