---
name: 01-setup-config
description: >
  Use this skill whenever a developer is starting a NEW Next.js + TypeScript project from scratch,
  or setting up the foundation of an existing project. This skill REPLACES using skills 01 and 08
  individually. Triggers include: "เริ่มต้น project", "สร้าง project ใหม่", "setup project",
  "boilerplate", "scaffold", "เริ่มจากศูนย์", "project structure", "ตั้งค่า project",
  "init project", "create next app". This skill chains all required skills automatically.
  Always use this skill FIRST before any other skill when starting a project.
---

# Skill 01: Project Setup

## Operating Stance

- **Installer ไม่ใช่ designer.** Phase นี้คือการวาง foundation — ไม่ตัดสินใจ business logic ใดๆ ทั้งนั้น
- **Checklist-driven.** ทำตามลำดับ Phase เท่านั้น ห้าม skip แม้จะดู "ไม่จำเป็น"
- **Spec-aware.** ทุกการตัดสินใจ (folder pattern, dependencies) ต้องอ้างอิง Frontend Spec จาก REQ — ไม่ใช่ assume เอง

---

## When NOT to use

- **Project มี Next.js อยู่แล้ว** → ใช้เฉพาะ Phase ที่ขาด อย่ารัน install ซ้ำ
- **แค่ถามเรื่อง folder structure** → ใช้ Skill 01 แทน
- **แค่ถามเรื่อง env** → ใช้ Skill 08 แทน
- **ยังไม่มี Frontend Spec** → กลับไปรัน Skill REQ ก่อน

---

## Refuse to proceed ถ้า

```
❌ ไม่มี Frontend Spec จาก REQ → ขอ spec ก่อน หรือรัน Skill REQ ก่อน
❌ ไม่รู้ project size (small/medium/large) → ถามก่อนเลือก folder pattern
❌ user ขอ skip Phase ใดๆ → อธิบายผลกระทบ แล้วถามยืนยันอีกครั้ง
```

---

## วิธีใช้ไฟล์ใน Skill นี้

อ่าน SKILL.md นี้ก่อน จากนั้น **view ไฟล์ตาม Phase ที่กำลังทำ** — ห้ามข้าม Phase

---

## ไฟล์ที่มีและเมื่อไหร่ควรอ่าน

### references/ — อ่านก่อนเริ่มเสมอ
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/dependencies.md` | Phase 1 — install commands ทั้งหมด |
| `references/project-checklist.md` | ทุก phase — checklist กันลืม |

### bad-patterns/ — อ่านก่อน Phase 2 เสมอ
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/setup-mistakes.md` | ก่อนสร้าง config — ป้องกัน mistake ที่พบบ่อย |

### templates/config-files/ — Phase 2: Config
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/config-files/next-config.md` | สร้าง next.config.js |
| `templates/config-files/eslint-prettier.md` | สร้าง .eslintrc.json + prettier.config.js |
| `templates/config-files/tailwind-postcss.md` | สร้าง tailwind.config.ts + postcss.config.js |
| `templates/config-files/gitignore.md` | สร้าง .gitignore |

### templates/ (Skill 01 + 04) — Phase 3: Folder + tsconfig
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `../02-folder-types/bad-patterns/common-mistakes.md` | อ่านก่อนสร้างไฟล์ทุกครั้ง |
| `../02-folder-types/templates/layer-folder.md` | scaffold folder แบบ layer (โปรเจกต์กลาง) |
| `../02-folder-types/templates/feature-folder.md` | scaffold folder แบบ feature (โปรเจกต์ใหญ่) |
| `../05-ts-zod/references/tsconfig-standard.md` | สร้าง tsconfig.json |

### templates/boilerplate/ — Phase 4: Boilerplate Files
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/boilerplate/env-ts.md` | สร้าง src/lib/env.ts + .env.example |
| `templates/boilerplate/utils-ts.md` | สร้าง src/lib/utils.ts |
| `templates/boilerplate/types.md` | สร้าง src/types/ ทั้งหมด |
| `templates/boilerplate/layout-providers.md` | สร้าง layout.tsx + providers.tsx |
| `templates/boilerplate/health-route.md` | สร้าง api/health/route.ts |

### templates/ — Phase 5: CLAUDE.md
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/claude-md.md` | สร้าง CLAUDE.md ที่ root |

---

## Output Format (บังคับทุกครั้ง)

1. **Phase 1** — แสดง install commands จาก `references/dependencies.md`
2. **Phase 2** — สร้าง config files ทีละไฟล์ พร้อมอธิบายสั้นๆ ว่าทำไม
3. **Phase 3** — scaffold folder + tsconfig
4. **Phase 4** — สร้าง boilerplate files ทีละไฟล์
5. **Phase 5** — สร้าง CLAUDE.md
6. **Phase 6** — แสดง verify commands (`npm run dev`, `npm run build`, `npx tsc --noEmit`)

---

## Worked Example

**หลัง Phase 2 ผ่าน ต้องได้:**
```
✅ next.config.js    — security headers + image domains
✅ .eslintrc.json    — exhaustive-deps เป็น "error"
✅ prettier.config.js — tailwindcss plugin
✅ tailwind.config.ts — content paths ครบ
✅ .gitignore        — ครอบ .env ทุกรูปแบบ
```

**หลัง Phase 4 ผ่าน ต้องได้:**
```
✅ src/lib/env.ts           — Zod schema validate env ก่อน runtime
✅ src/lib/utils.ts         — cn(), formatCurrency(), formatDate()
✅ src/types/index.ts       — re-export hub
✅ src/types/api.types.ts   — ApiSuccess, ApiError, PaginatedResponse
✅ src/app/providers.tsx    — QueryClientProvider wrapper
✅ src/app/layout.tsx       — font + metadata + Providers
✅ src/app/api/health/route.ts
✅ .env.example
```

**Phase 6 Verify — ห้ามประกาศเสร็จจนกว่าทุกข้อผ่าน:**
```
□ npm run dev    → ขึ้น localhost:3000 ไม่มี error
□ npm run build  → สำเร็จ ไม่มี error
□ npx tsc --noEmit → 0 errors
□ npm run lint   → 0 errors (ไม่ใช่แค่ warn)
□ git status     → .env ไม่อยู่ใน tracked files
```

---

## Behavioral Rules

```
[ONE-PHASE-AT-A-TIME]   ทำทีละ Phase แสดงผล Phase นั้นให้ครบก่อนไป Phase ถัดไป
[VERIFY-BEFORE-DONE]    ต้องรัน verify commands ก่อนประกาศ DONE ทุกครั้ง
[NO-SILENT-SKIP]        ถ้า Phase ไม่เกี่ยว → ประกาศ SKIPPED พร้อมเหตุผล อย่าเงียบ
[LOG-UPDATE]            หลังทำการเปลี่ยนแปลงใดๆ ใน project → append entry ลง LOG.md ทันที
                        format: Bug Fix entry ถ้าแก้ bug / Phase entry ถ้าเป็นส่วนหนึ่งของ _start
                        ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill REQ → รับ Frontend Spec เป็น input
ถัดไป:    start-all-project Phase 2 (Skill 01 + 04) → Folder Structure + TypeScript
```
