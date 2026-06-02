# Template: Dynamic Imports (copy ได้เลย)

```tsx
import dynamic from 'next/dynamic'

// Heavy library — โหลดเฉพาะตอนใช้
const HeavyChart = dynamic(
  () => import('@components/features/analytics/HeavyChart'),
  { loading: () => <ChartSkeleton />, ssr: false }
)

// Component ที่ใช้ browser API
const RichTextEditor = dynamic(
  () => import('@components/ui/RichTextEditor'),
  { loading: () => <div>Loading editor...</div>, ssr: false }
)

// Named export
const { SpecificComponent } = dynamic(
  () => import('@components/ui/MultiExport').then(m => ({ default: m.SpecificComponent })),
  { ssr: false }
)

// Conditional render — โหลดเฉพาะตอน condition true
function BlogPost() {
  const [showEditor, setShowEditor] = useState(false)
  return (
    <div>
      <button onClick={() => setShowEditor(true)}>Edit</button>
      {showEditor && <RichTextEditor />}
    </div>
  )
}
```
