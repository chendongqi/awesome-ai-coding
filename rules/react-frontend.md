# React Frontend Development Rules

## Component Structure (CRITICAL)

ALWAYS use functional components with hooks:

```tsx
// WRONG: Class component
class UserProfile extends React.Component {
    state = { user: null }
    
    componentDidMount() {
        fetchUser().then(user => this.setState({ user }))
    }
    
    render() {
        return <div>{this.state.user?.name}</div>
    }
}

// CORRECT: Functional component with hooks
function UserProfile({ userId }: { userId: string }) {
    const [user, setUser] = useState<User | null>(null)
    const [loading, setLoading] = useState(true)
    
    useEffect(() => {
        fetchUser(userId)
            .then(setUser)
            .finally(() => setLoading(false))
    }, [userId])
    
    if (loading) return <Spinner />
    if (!user) return <div>User not found</div>
    
    return <div>{user.name}</div>
}
```

## State Management (MANDATORY)

ALWAYS use proper state management:

```tsx
// WRONG: Prop drilling
function App() {
    const [user, setUser] = useState(null)
    return <Page user={user} setUser={setUser} />
}

function Page({ user, setUser }) {
    return <Header user={user} setUser={setUser} />
}

// CORRECT: Context API or state management library
const UserContext = createContext<UserContextType | null>(null)

function App() {
    const [user, setUser] = useState<User | null>(null)
    
    return (
        <UserContext.Provider value={{ user, setUser }}>
            <Page />
        </UserContext.Provider>
    )
}

function Page() {
    return <Header />
}

function Header() {
    const { user } = useContext(UserContext)!
    return <div>{user?.name}</div>
}
```

## Immutability (CRITICAL)

ALWAYS create new objects, NEVER mutate:

```tsx
// WRONG: Mutation
function updateUser(users: User[], id: string, name: string) {
    const user = users.find(u => u.id === id)
    user.name = name  // MUTATION!
    return users
}

// CORRECT: Immutability
function updateUser(users: User[], id: string, name: string) {
    return users.map(user =>
        user.id === id ? { ...user, name } : user
    )
}
```

## Custom Hooks (MANDATORY)

ALWAYS extract reusable logic into custom hooks:

```tsx
// CORRECT: Custom hook
function useUser(userId: string) {
    const [user, setUser] = useState<User | null>(null)
    const [loading, setLoading] = useState(true)
    const [error, setError] = useState<Error | null>(null)
    
    useEffect(() => {
        setLoading(true)
        setError(null)
        
        fetchUser(userId)
            .then(setUser)
            .catch(setError)
            .finally(() => setLoading(false))
    }, [userId])
    
    return { user, loading, error }
}

// Usage
function UserProfile({ userId }: { userId: string }) {
    const { user, loading, error } = useUser(userId)
    
    if (loading) return <Spinner />
    if (error) return <ErrorMessage error={error} />
    if (!user) return <div>User not found</div>
    
    return <div>{user.name}</div>
}
```

## Performance Optimization (CRITICAL)

ALWAYS optimize re-renders:

```tsx
// WRONG: Unnecessary re-renders
function ExpensiveComponent({ data }: { data: Data[] }) {
    const processed = data.map(/* expensive operation */)
    return <div>{processed}</div>
}

// CORRECT: Memoization
const ExpensiveComponent = memo(function ExpensiveComponent({ 
    data 
}: { 
    data: Data[] 
}) {
    const processed = useMemo(
        () => data.map(/* expensive operation */),
        [data]
    )
    return <div>{processed}</div>
})

// CORRECT: Callback memoization
function Parent() {
    const [count, setCount] = useState(0)
    
    const handleClick = useCallback(() => {
        console.log('Clicked')
    }, [])
    
    return <Child onClick={handleClick} />
}
```

## Error Boundaries (MANDATORY)

ALWAYS use error boundaries:

```tsx
// CORRECT: Error boundary
class ErrorBoundary extends React.Component<
    { children: React.ReactNode },
    { hasError: boolean; error?: Error }
> {
    constructor(props: { children: React.ReactNode }) {
        super(props)
        this.state = { hasError: false }
    }
    
    static getDerivedStateFromError(error: Error) {
        return { hasError: true, error }
    }
    
    componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
        console.error('Error caught:', error, errorInfo)
    }
    
    render() {
        if (this.state.hasError) {
            return <ErrorFallback error={this.state.error} />
        }
        return this.props.children
    }
}
```

## TypeScript (CRITICAL)

ALWAYS use TypeScript with strict types:

```tsx
// WRONG: No types
function UserCard(props) {
    return <div>{props.name}</div>
}

// CORRECT: Strict types
interface UserCardProps {
    user: User
    onEdit?: (user: User) => void
}

function UserCard({ user, onEdit }: UserCardProps) {
    return (
        <div>
            <h2>{user.name}</h2>
            {onEdit && (
                <button onClick={() => onEdit(user)}>Edit</button>
            )}
        </div>
    )
}
```

## Code Quality Checklist

Before marking work complete:
- [ ] Functional components with hooks (no class components)
- [ ] No prop drilling (use Context or state management)
- [ ] Immutable state updates
- [ ] Custom hooks for reusable logic
- [ ] Memoization for expensive operations
- [ ] Error boundaries implemented
- [ ] TypeScript strict mode enabled
- [ ] No console.log statements
- [ ] Proper error handling
- [ ] Accessibility (a11y) attributes
