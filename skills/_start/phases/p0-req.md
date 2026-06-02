# Phase 0 — REQ: Requirements Analysis

**ไฟล์ที่ต้องอ่าน:**
```
../00-req-spec/references/question-bank.md
../00-req-spec/references/decision-defaults.md
../00-req-spec/bad-patterns/assumption-traps.md
../00-req-spec/templates/spec-output.md
../00-req-spec/templates/ambiguity-checklist.md
```

**ขั้นตอน:**
1. จำแนก project type (e-commerce, SaaS, internal tool, content site, ฯลฯ)
2. โหลดคำถามจาก `question-bank.md` — **ถาม Data Mode ก่อนเสมอ** (🔴 Blocker อันดับแรก):
   - Mock Data / Real API / Mixed?
   - ถ้า Real/Mixed: ขอ Backend URL + API docs
3. หา Ambiguity ทุกจุด แยก 🔴 Blocker / 🟡 Important / 🟢 Minor
4. สร้าง Frontend Spec ครบทุก section จาก `spec-output.md`
   - section "API Integration Mode" บันทึก Data Mode + URL
   - section "Backend Connection Guide" ถ้า Real/Mixed — list ทุก endpoint พร้อม path
5. สร้าง Ambiguity List จาก `ambiguity-checklist.md`

**🚨 GATE 0:**
- ถ้ามี 🔴 Blocker → **หยุดทันที** แสดง Ambiguity List ให้ผู้ใช้ตอบ
- ห้าม proceed Phase 1 จนกว่า Blocker ทุกข้อจะได้รับคำตอบ
- ✅ ผ่าน Gate เมื่อ: ไม่มี 🔴 Blocker ค้างอยู่ + Frontend Spec สมบูรณ์

**📋 LOG.md — ทำก่อนเริ่ม Phase 0:**
1. สร้างไฟล์ `LOG.md` ที่ root ของ project โดยใช้ template จาก `templates/activity-log.md`
2. กรอก Project Info (name, date)

**📋 LOG.md — append entry หลัง Gate ผ่าน:**
```
## [001] YYYY-MM-DD | _start — Phase 0: REQ Analysis
**Type**: Phase
**Skill Used**: _start + 00-req-spec
**Action**: วิเคราะห์ requirement สร้าง Frontend Spec
**Decisions**:
- Data Mode: [Mock / Real API / Mixed] — เหตุผล: [reason]
- API Base URL: [URL หรือ N/A]
- [decisions อื่นๆ]
**Blockers Found**: [list หรือ none]
**Gate**: ✅
```
