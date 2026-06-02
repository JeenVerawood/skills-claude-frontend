# Template: Async useEffect พร้อม Cleanup

## Pattern 1: cancelled flag (simple)

```tsx
useEffect(() => {
  let cancelled = false

  const load = async () => {
    try {
      const data = await fetchUser(userId)
      if (!cancelled) setUser(data)
    } catch (err) {
      if (!cancelled) setError(err)
    }
  }

  load()
  return () => { cancelled = true }
}, [userId])
```

## Pattern 2: AbortController (แนะนำสำหรับ fetch)

```tsx
useEffect(() => {
  const controller = new AbortController()

  const load = async () => {
    try {
      const res = await fetch(`/api/user/${userId}`, {
        signal: controller.signal,
      })
      const data = await res.json()
      setUser(data)
    } catch (err) {
      if (err instanceof Error && err.name !== 'AbortError') {
        setError(err.message)
      }
    }
  }

  load()
  return () => controller.abort()
}, [userId])
```

## Pattern 3: Full loading state

```tsx
const [state, setState] = useState<{
  data: User | null
  loading: boolean
  error: string | null
}>({ data: null, loading: true, error: null })

useEffect(() => {
  const controller = new AbortController()

  setState(prev => ({ ...prev, loading: true, error: null }))

  fetch(`/api/user/${userId}`, { signal: controller.signal })
    .then(r => r.json())
    .then(data => setState({ data, loading: false, error: null }))
    .catch(err => {
      if (err.name !== 'AbortError') {
        setState({ data: null, loading: false, error: err.message })
      }
    })

  return () => controller.abort()
}, [userId])
```
