# Decision Defaults — ใช้เมื่อลูกค้าไม่ได้ตอบ

## วิธีใช้

ถ้า ambiguity item เป็นระดับ 🟢 Minor → ไม่ต้องถามลูกค้า
ใช้ default ด้านล่างแทน แต่ **ต้องระบุใน spec ว่า assumed อะไร**

Format: `[ASSUMED] ใช้ X เพราะ Y — เปลี่ยนได้ถ้าลูกค้าต้องการอื่น`

---

## Data Mode Default

| Decision | Default | เหตุผล |
|---|---|---|
| Data Mode | Mixed (Mock → Real) | ทำให้ Frontend ทำงานได้ทันทีโดยไม่รอ Backend ใช้ env var switch |
| Mock ตัวเลือก | ใช้ in-route mock ใน dev mode เท่านั้น | ไม่ปน production code |
| Switch condition | ตั้ง `API_BASE_URL` ใน `.env.local` | mock หายอัตโนมัติเมื่อมีค่า |

```
[ASSUMED] ใช้ Mixed mode (Mock ก่อน → Real ด้วย env var)
เพราะ: ทำให้ Frontend dev ไม่ต้องรอ Backend พร้อม
เปลี่ยนได้: ถ้าลูกค้าต้องการ Real API ทันที → ลบ mock code ออก
```

## Auth Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Session duration | 7 วัน | balance ระหว่าง UX กับ security |
| Token storage | httpOnly cookie | ป้องกัน XSS ดีกว่า localStorage |
| Password min length | 8 ตัวอักษร | NIST guideline |
| Max login attempts | 5 ครั้ง / 15 นาที | ป้องกัน brute force |
| Email verification | required | ป้องกัน fake accounts |

## Database Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Soft delete | ใช้ `deletedAt` field | ป้องกันข้อมูลหายถาวรโดยไม่ตั้งใจ |
| Timestamps | `createdAt` + `updatedAt` ทุก table | audit trail พื้นฐาน |
| UUID vs integer ID | UUID (cuid2) | ป้องกัน enumeration attack |
| Pagination default size | 20 items/page | balance ระหว่าง performance กับ UX |
| Pagination type | cursor-based | scale ได้ดีกว่า offset pagination |

## API Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Error format | `{ error: string, code: string }` | consistent ทั่ว app |
| Success format | `{ data: T, message?: string }` | predictable |
| Rate limiting | 100 req/min per IP | ป้องกัน abuse เบื้องต้น |
| API versioning | `/api/v1/` | รองรับ breaking change ในอนาคต |
| CORS | origin whitelist | ไม่ใช่ `*` |

## File Upload Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Image storage | Uploadthing | setup ง่าย, integrate กับ Next.js ได้ดี |
| Max file size | 10MB | พอสำหรับรูปสัตว์ quality ดี |
| Allowed types | jpg, jpeg, png, webp | ครอบ use case ทั่วไป |
| Image optimization | Next.js Image component | built-in, ไม่ต้องตั้งค่าเพิ่ม |

## Frontend Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Loading state | Skeleton screen | ดีกว่า spinner สำหรับ content-heavy |
| Error display | Toast notification | ไม่ block UI |
| Empty state | Illustrated empty state + CTA | UX ดีกว่า แค่ "ไม่มีข้อมูล" |
| Form validation | Client + Server (Zod both sides) | UX fast + secure |
| Date format | DD/MM/YYYY (Thai) | เหมาะกับ Thai market |
| Currency format | ฿ X,XXX (no decimal) | standard Thai e-commerce |

## Notification Defaults

| Decision | Default | เหตุผล |
|---|---|---|
| Notification channel | Email only (phase 1) | พอสำหรับ MVP |
| Email provider | Resend | DX ดีที่สุดสำหรับ Next.js |
| Push notification | Phase 2 (ไม่รวมใน MVP) | ลด complexity เริ่มต้น |

---

## Defaults ที่ห้ามใช้โดยไม่ถาม (ต้อง confirm เสมอ)

```
❌ Payment gateway — ต้องรู้ก่อนว่าลูกค้า approve vendor ไหนแล้ว
❌ Third-party analytics — อาจมี privacy policy ที่ต้องคุยกับลูกค้า
❌ Data retention period — อาจมีกฎหมายที่เกี่ยวข้อง (PDPA)
❌ Geographic restrictions — ต้องรู้ว่าให้บริการที่ไหน
❌ Multi-language — ต้องรู้ scope ก่อนทำ
```
