# Template: Context + Custom Hook (copy ได้เลย)

```tsx
// src/contexts/AuthContext.tsx
'use client'
import { createContext, useContext, useState, useCallback, useMemo } from 'react'

// 1. Types
type User = { id: string; name: string; role: 'admin' | 'user' }
type AuthContextType = {
  user: User | null
  login: (email: string, password: string) => Promise<void>
  logout: () => void
  isLoading: boolean
}

// 2. Context
const AuthContext = createContext<AuthContextType | null>(null)

// 3. Provider
export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null)
  const [isLoading, setIsLoading] = useState(false)

  const login = useCallback(async (email: string, password: string) => {
    setIsLoading(true)
    try {
      const res = await fetch('/api/auth/login', {
        method: 'POST',
        body: JSON.stringify({ email, password }),
      })
      const data = await res.json()
      setUser(data.user)
    } finally {
      setIsLoading(false)
    }
  }, [])

  const logout = useCallback(() => {
    fetch('/api/auth/logout', { method: 'DELETE' })
    setUser(null)
  }, [])

  // useMemo ป้องกัน re-render ทั้ง tree
  const value = useMemo(
    () => ({ user, login, logout, isLoading }),
    [user, login, logout, isLoading]
  )

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>
}

// 4. Custom hook — ใช้แทน useContext ตรงๆ เสมอ
export function useAuth() {
  const ctx = useContext(AuthContext)
  if (!ctx) throw new Error('useAuth must be used within <AuthProvider>')
  return ctx
}
```

## การใช้งาน

```tsx
// app/layout.tsx
import { AuthProvider } from '@lib/contexts/AuthContext'

export default function RootLayout({ children }) {
  return <AuthProvider>{children}</AuthProvider>
}

// ใน component
import { useAuth } from '@lib/contexts/AuthContext'

function ProfileButton() {
  const { user, logout } = useAuth()
  return user ? <button onClick={logout}>{user.name}</button> : null
}
```
