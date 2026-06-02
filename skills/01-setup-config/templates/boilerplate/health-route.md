# src/app/api/health/route.ts (copy ได้เลย)

```ts
// Health check endpoint สำหรับ frontend Next.js app
// ใช้กับ: Docker HEALTHCHECK, monitoring (Uptime Robot, etc.), load balancer

export async function GET() {
  return Response.json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    version: process.env.npm_package_version ?? '0.0.0',
    env: process.env.NODE_ENV,
  })
}
```

## ใช้กับ Docker Compose

```yaml
healthcheck:
  test: ['CMD', 'curl', '-f', 'http://localhost:3000/api/health']
  interval: 30s
  timeout: 10s
  retries: 3
```
