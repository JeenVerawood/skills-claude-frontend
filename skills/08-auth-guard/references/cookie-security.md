# Cookie Security

## Attributes มาตรฐาน (ต้องครบทุกตัว)

```ts
response.cookies.set('auth-token', token, {
  httpOnly: true,    // JS อ่านไม่ได้ → ป้องกัน XSS
  secure:   process.env.NODE_ENV === 'production',  // HTTPS only ใน production
  sameSite: 'lax',  // ป้องกัน CSRF (ใช้ 'strict' ถ้าไม่ต้อง cross-site)
  maxAge:   60 * 60 * 24 * 7,  // 7 days
  path:     '/',
})
```

## sameSite เลือกแบบไหน

```
'lax'    → แนะนำสำหรับส่วนใหญ่ (ป้องกัน CSRF, รองรับ OAuth redirect)
'strict' → เข้มงวดที่สุด (อาจมีปัญหากับ OAuth หรือ third-party links)
'none'   → ต้องใช้กับ secure: true เท่านั้น (สำหรับ embedded iframe)
```

## JWT Storage — เปรียบเทียบ

```
localStorage  ❌ XSS อ่านได้, ใช้ JS โจมตีได้ง่าย
sessionStorage ❌ เหมือน localStorage แต่หมดเมื่อปิด tab
httpOnly cookie ✅ JS อ่านไม่ได้, ป้องกัน XSS
```
