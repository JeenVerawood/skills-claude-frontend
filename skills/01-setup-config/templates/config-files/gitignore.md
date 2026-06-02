# .gitignore มาตรฐาน (copy ได้เลย)

```
# Next.js
.next/
out/
build/
*.tsbuildinfo
next-env.d.ts

# Node
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Env — ห้าม commit ทุกกรณีไม่มีข้อยกเว้น
.env
.env.local
.env.development
.env.staging
.env.production
.env.*.local

# OS
.DS_Store
Thumbs.db
Desktop.ini

# Editor
.vscode/settings.json
.idea/
*.suo
*.ntvs*
*.njsproj
*.sln

# Testing
coverage/
.vitest-cache/

# Misc
*.log
.vercel
```

## ⚠️ ต้องสร้างก่อน commit แรกเสมอ

```bash
# ลำดับที่ถูกต้อง
git init
# สร้าง .gitignore ← ก่อน
git add .
git commit -m "initial commit"

# ❌ ห้าม: git add . ก่อนมี .gitignore
# → อาจ commit .env ไปโดยไม่รู้ตัว
```
