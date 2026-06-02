# tsconfig.json มาตรฐาน

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "baseUrl": ".",
    "paths": {
      "@/*":           ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@hooks/*":      ["./src/hooks/*"],
      "@types/*":      ["./src/types/*"],
      "@lib/*":        ["./src/lib/*"],
      "@constants/*":  ["./src/constants/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## Options สำคัญที่ต้องเปิดเสมอ

```
strict: true              → ครอบ strictNullChecks, noImplicitAny, strictFunctionTypes
noUncheckedIndexedAccess  → array[i] ได้ T | undefined (ปลอดภัยขึ้น)
noImplicitReturns         → function ต้อง return ทุก code path
```

## เพิ่ม test environment

```json
{
  "compilerOptions": {
    "types": ["vitest/globals", "@testing-library/jest-dom"]
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/test/**/*.ts"]
}
```
