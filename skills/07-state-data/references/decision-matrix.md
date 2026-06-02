# State Management Decision Matrix

## เลือกจากตารางนี้เสมอ — อ่านก่อนตอบทุกครั้ง

```
ประเภท State                           Tool
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Server data (ดึงจาก API)              TanStack Query / SWR
Form state                            React Hook Form + Zod
URL / Filter / Pagination state       useSearchParams + router.push
Local component state                 useState
Shared UI state (modal, theme, cart)  Zustand
Auth state                            Context หรือ Zustand
Complex global (หลาย slice)          Zustand (แบ่ง store)
```

## กฎ

- ห้ามเก็บ server data ใน Zustand/Redux แล้ว sync เอง → ใช้ TanStack Query
- URL state ดีกว่า global state สำหรับ filter/pagination (shareable, bookmarkable)
- useState ก่อน — upgrade เมื่อจำเป็นจริงๆ เท่านั้น
