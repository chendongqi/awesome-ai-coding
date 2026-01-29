# Backend Development Rules

## API Design (CRITICAL)

ALWAYS follow RESTful conventions:

```typescript
// CORRECT: RESTful API structure
// GET /api/users - List users
// GET /api/users/:id - Get user
// POST /api/users - Create user
// PUT /api/users/:id - Update user
// DELETE /api/users/:id - Delete user

interface ApiResponse<T> {
    success: boolean
    data?: T
    error?: {
        code: string
        message: string
        details?: unknown
    }
    meta?: {
        total?: number
        page?: number
        limit?: number
    }
}
```

## Input Validation (MANDATORY)

ALWAYS validate all inputs:

```typescript
// CORRECT: Zod schema validation
import { z } from 'zod'

const createUserSchema = z.object({
    email: z.string().email(),
    name: z.string().min(1).max(100),
    age: z.number().int().min(0).max(150),
    role: z.enum(['user', 'admin', 'moderator'])
})

async function createUser(req: Request, res: Response) {
    try {
        const validated = createUserSchema.parse(req.body)
        const user = await userService.create(validated)
        res.json({ success: true, data: user })
    } catch (error) {
        if (error instanceof z.ZodError) {
            res.status(400).json({
                success: false,
                error: {
                    code: 'VALIDATION_ERROR',
                    message: 'Invalid input',
                    details: error.errors
                }
            })
        } else {
            res.status(500).json({
                success: false,
                error: {
                    code: 'INTERNAL_ERROR',
                    message: 'Internal server error'
                }
            })
        }
    }
}
```

## Error Handling (CRITICAL)

ALWAYS handle errors comprehensively:

```typescript
// CORRECT: Centralized error handling
class AppError extends Error {
    constructor(
        public code: string,
        message: string,
        public statusCode: number = 500,
        public details?: unknown
    ) {
        super(message)
        this.name = 'AppError'
    }
}

class NotFoundError extends AppError {
    constructor(resource: string, id: string) {
        super(
            'NOT_FOUND',
            `${resource} with id ${id} not found`,
            404
        )
    }
}

// Error handler middleware
function errorHandler(
    err: Error,
    req: Request,
    res: Response,
    next: NextFunction
) {
    if (err instanceof AppError) {
        res.status(err.statusCode).json({
            success: false,
            error: {
                code: err.code,
                message: err.message,
                details: err.details
            }
        })
    } else {
        console.error('Unexpected error:', err)
        res.status(500).json({
            success: false,
            error: {
                code: 'INTERNAL_ERROR',
                message: 'Internal server error'
            }
        })
    }
}
```

## Database Operations (MANDATORY)

ALWAYS use parameterized queries:

```typescript
// WRONG: SQL injection risk
const query = `SELECT * FROM users WHERE id = ${userId}`

// CORRECT: Parameterized queries
const query = 'SELECT * FROM users WHERE id = ?'
const result = await db.query(query, [userId])

// CORRECT: ORM with parameterized queries
const user = await prisma.user.findUnique({
    where: { id: userId }
})
```

## Authentication & Authorization (CRITICAL)

ALWAYS implement proper auth:

```typescript
// CORRECT: JWT authentication middleware
import jwt from 'jsonwebtoken'

interface AuthRequest extends Request {
    user?: {
        id: string
        email: string
        role: string
    }
}

function authenticateToken(
    req: AuthRequest,
    res: Response,
    next: NextFunction
) {
    const authHeader = req.headers['authorization']
    const token = authHeader?.split(' ')[1]
    
    if (!token) {
        return res.status(401).json({
            success: false,
            error: { code: 'UNAUTHORIZED', message: 'No token provided' }
        })
    }
    
    jwt.verify(token, process.env.JWT_SECRET!, (err, user) => {
        if (err) {
            return res.status(403).json({
                success: false,
                error: { code: 'FORBIDDEN', message: 'Invalid token' }
            })
        }
        req.user = user as AuthRequest['user']
        next()
    })
}

// CORRECT: Role-based authorization
function requireRole(...roles: string[]) {
    return (req: AuthRequest, res: Response, next: NextFunction) => {
        if (!req.user || !roles.includes(req.user.role)) {
            return res.status(403).json({
                success: false,
                error: { code: 'FORBIDDEN', message: 'Insufficient permissions' }
            })
        }
        next()
    }
}
```

## Rate Limiting (MANDATORY)

ALWAYS implement rate limiting:

```typescript
// CORRECT: Rate limiting middleware
import rateLimit from 'express-rate-limit'

const apiLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // Limit each IP to 100 requests per windowMs
    message: {
        success: false,
        error: {
            code: 'RATE_LIMIT_EXCEEDED',
            message: 'Too many requests, please try again later'
        }
    }
})

app.use('/api/', apiLimiter)
```

## Logging (CRITICAL)

ALWAYS use structured logging:

```typescript
// CORRECT: Structured logging
import winston from 'winston'

const logger = winston.createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: winston.format.json(),
    transports: [
        new winston.transports.File({ filename: 'error.log', level: 'error' }),
        new winston.transports.File({ filename: 'combined.log' })
    ]
})

logger.info('User created', {
    userId: user.id,
    email: user.email,
    timestamp: new Date().toISOString()
})
```

## Code Quality Checklist

Before marking work complete:
- [ ] RESTful API design followed
- [ ] All inputs validated (Zod/Yup schemas)
- [ ] Parameterized database queries (no SQL injection)
- [ ] Authentication middleware implemented
- [ ] Authorization checks in place
- [ ] Rate limiting enabled
- [ ] Structured logging implemented
- [ ] Error handling middleware
- [ ] No hardcoded secrets
- [ ] Environment variables for configuration
- [ ] API response format consistent
