# RSC Optimization — Server vs Client

## กฎ: 'use client' ต้องอยู่ใน leaf component เล็กๆ เท่านั้น

```tsx
// ❌ use client ครอบ component ใหญ่ → ทั้ง tree กลายเป็น client bundle
'use client'
export default function DashboardPage() {
  const [tab, setTab] = useState('overview')  // ต้องการ client
  return (
    <div>
      <TabBar tab={tab} onChange={setTab} />
      <StatsSection />    {/* Server Component แต่กลายเป็น client เพราะ parent */}
      <ChartSection />    {/* เสีย RSC benefit ทั้งหมด */}
    </div>
  )
}

// ✅ แยก client state เป็น leaf component
// components/features/dashboard/TabController.tsx
'use client'
export function TabController({ children }: { children: (tab: string) => React.ReactNode }) {
  const [tab, setTab] = useState('overview')
  return (
    <div>
      <TabBar tab={tab} onChange={setTab} />
      {children(tab)}
    </div>
  )
}

// app/dashboard/page.tsx — Server Component
export default async function DashboardPage() {
  const [stats, chartData] = await Promise.all([fetchStats(), fetchChart()])
  return (
    <TabController>
      {(tab) => (
        <>
          {tab === 'overview'   && <StatsSection data={stats} />}
          {tab === 'analytics'  && <ChartSection data={chartData} />}
        </>
      )}
    </TabController>
  )
}
```

## Checklist

```
□ ทุก 'use client' เป็น leaf component
□ layout.tsx และ page.tsx ไม่มี 'use client'
□ fetch ข้อมูลใน Server Component (ไม่ใช้ useEffect)
□ Interactive UI เล็กๆ แยกเป็น component แล้ว 'use client'
```
