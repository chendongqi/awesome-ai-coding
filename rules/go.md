# Go Development Rules

## Code Style (CRITICAL)

ALWAYS follow Go conventions:

```go
// WRONG: Non-idiomatic naming
func GetUserData() {
    var UserName string
    var user_age int
}

// CORRECT: Go naming conventions
func GetUserData() {
    var userName string  // camelCase for unexported
    var userAge int
}

// CORRECT: Exported functions use PascalCase
type UserService struct {
    db *Database  // unexported field
}

func (s *UserService) GetUser(id string) (*User, error) {
    // Exported method
}
```

## Error Handling (MANDATORY)

ALWAYS return errors explicitly:

```go
// WRONG: Ignoring errors
result, _ := riskyOperation()

// CORRECT: Always handle errors
result, err := riskyOperation()
if err != nil {
    return nil, fmt.Errorf("operation failed: %w", err)
}

// CORRECT: Error wrapping with context
func processUser(id string) (*User, error) {
    user, err := db.GetUser(id)
    if err != nil {
        return nil, fmt.Errorf("failed to get user %s: %w", id, err)
    }
    return user, nil
}
```

## Context Usage (CRITICAL)

ALWAYS pass context.Context for cancellable operations:

```go
// WRONG: No context
func fetchData(url string) ([]byte, error) {
    resp, err := http.Get(url)  // No cancellation!
    // ...
}

// CORRECT: Context-aware
func fetchData(ctx context.Context, url string) ([]byte, error) {
    req, err := http.NewRequestWithContext(ctx, "GET", url, nil)
    if err != nil {
        return nil, err
    }
    
    resp, err := http.DefaultClient.Do(req)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()
    
    return io.ReadAll(resp.Body)
}
```

## Interfaces (MANDATORY)

PREFER small, focused interfaces:

```go
// WRONG: Large interface
type UserService interface {
    GetUser(id string) (*User, error)
    CreateUser(user *User) error
    UpdateUser(user *User) error
    DeleteUser(id string) error
    ListUsers() ([]*User, error)
    SearchUsers(query string) ([]*User, error)
    // ... 20 more methods
}

// CORRECT: Small, focused interfaces
type UserReader interface {
    GetUser(id string) (*User, error)
    ListUsers() ([]*User, error)
}

type UserWriter interface {
    CreateUser(user *User) error
    UpdateUser(user *User) error
    DeleteUser(id string) error
}

type UserService interface {
    UserReader
    UserWriter
}
```

## Concurrency (CRITICAL)

ALWAYS use proper synchronization:

```go
// WRONG: Race condition
var counter int

func increment() {
    counter++  // Race condition!
}

// CORRECT: Mutex protection
type SafeCounter struct {
    mu    sync.RWMutex
    value int
}

func (c *SafeCounter) Increment() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.value++
}

func (c *SafeCounter) Value() int {
    c.mu.RLock()
    defer c.mu.RUnlock()
    return c.value
}

// CORRECT: Channels for communication
func worker(jobs <-chan int, results chan<- int) {
    for job := range jobs {
        results <- job * 2
    }
}
```

## Resource Management (MANDATORY)

ALWAYS close resources:

```go
// WRONG: Resource leak
func readFile(filename string) ([]byte, error) {
    file, err := os.Open(filename)
    // Forgot to close!
    return io.ReadAll(file)
}

// CORRECT: Defer cleanup
func readFile(filename string) ([]byte, error) {
    file, err := os.Open(filename)
    if err != nil {
        return nil, err
    }
    defer file.Close()  // Always closes
    
    return io.ReadAll(file)
}
```

## Testing (CRITICAL)

ALWAYS write table-driven tests:

```go
// CORRECT: Table-driven tests
func TestParseUser(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        want    *User
        wantErr bool
    }{
        {
            name:    "valid user",
            input:   `{"id":"1","name":"John"}`,
            want:    &User{ID: "1", Name: "John"},
            wantErr: false,
        },
        {
            name:    "invalid json",
            input:   `{invalid}`,
            want:    nil,
            wantErr: true,
        },
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := ParseUser(tt.input)
            if (err != nil) != tt.wantErr {
                t.Errorf("ParseUser() error = %v, wantErr %v", err, tt.wantErr)
                return
            }
            if !reflect.DeepEqual(got, tt.want) {
                t.Errorf("ParseUser() = %v, want %v", got, tt.want)
            }
        })
    }
}
```

## Code Quality Checklist

Before marking work complete:
- [ ] Go naming conventions followed
- [ ] All errors explicitly returned and handled
- [ ] Context.Context used for cancellable operations
- [ ] Small, focused interfaces
- [ ] Proper synchronization (mutex/channels)
- [ ] Resources closed with defer
- [ ] Table-driven tests written
- [ ] No global variables
- [ ] No panic() in library code
- [ ] gofmt/goimports applied
