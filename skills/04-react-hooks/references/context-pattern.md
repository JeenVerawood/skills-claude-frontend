# Context Pattern

## เมื่อไหรใช้ Context

```
1-2 ชั้น  → useState + props ปกติ
3+ ชั้น   → Context หรือ Zustand
```

## Anti-pattern ที่ต้องหลีกเลี่ยง

```tsx
// ❌ value object ใหม่ทุก render → ทุก consumer re-render
<ThemeContext.Provider value={{ theme, setTheme }}>

// ✅ useMemo ครอบ value
const value = useMemo(() => ({ theme, setTheme }), [theme])
<ThemeContext.Provider value={value}>
```

## เมื่อไหรใช้ Zustand แทน Context

```
Context         → auth state, theme, locale (เปลี่ยนไม่บ่อย)
Zustand         → cart, UI state, ข้อมูลที่ update บ่อย
TanStack Query  → server data ทุกกรณี (ไม่ใช้ Context/Zustand)
```

→ ดู template สำเร็จรูปใน `templates/context-provider.md`
