# Phase 7 — Skill 03: React Patterns Audit

**Summary Phase 6 ก่อนเริ่ม** (API routes + state tools ที่ใช้)

**ไฟล์ที่ต้องอ่าน:**
```
../04-react-hooks/references/useeffect-rules.md
../04-react-hooks/references/memo-callback.md
../04-react-hooks/references/state-vs-ref.md
../04-react-hooks/references/context-pattern.md
../04-react-hooks/bad-patterns/useeffect-mistakes.md
../04-react-hooks/bad-patterns/rerender-mistakes.md
```

**ขั้นตอน:**
1. Audit components ที่สร้างจาก Phase 6 — ตรวจ useEffect dependency array
2. ตรวจ re-render pattern — useCallback / useMemo ใส่ถูกที่หรือไม่
3. แก้ anti-patterns ที่พบทุกจุด พร้อมอธิบาย ❌ → ✅

**✅ GATE 7:** ไม่มี useEffect anti-pattern (infinite loop, stale closure, memory leak) ก่อนไป Phase 8

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [NNN] YYYY-MM-DD | _start — Phase 7: React Patterns Audit
**Type**: Phase
**Skill Used**: _start + 04-react-hooks
**Action**: audit และแก้ React anti-patterns
**Files Modified**:
- [component path] — [อธิบาย anti-pattern ที่แก้]
**Notes**: [list fixes หรือ "none found"]
**Gate**: ✅
```
