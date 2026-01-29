# Node.js Development Rules

## Async Patterns (CRITICAL)

ALWAYS use async/await, NEVER use callbacks:

```javascript
// WRONG: Callback hell
function fetchData(callback) {
    fs.readFile('data.json', (err, data) => {
        if (err) return callback(err)
        const parsed = JSON.parse(data)
        processData(parsed, (err, result) => {
            callback(err, result)
        })
    })
}

// CORRECT: Async/await
import { promises as fs } from 'fs'

async function fetchData() {
    try {
        const data = await fs.readFile('data.json', 'utf-8')
        const parsed = JSON.parse(data)
        return await processData(parsed)
    } catch (error) {
        throw new Error(`Failed to fetch data: ${error.message}`)
    }
}
```

## Error Handling (MANDATORY)

ALWAYS handle errors properly:

```javascript
// WRONG: Unhandled promise rejection
async function riskyOperation() {
    await someAsyncCall()  // May throw!
}

riskyOperation()  // Unhandled rejection!

// CORRECT: Try-catch
async function riskyOperation() {
    try {
        await someAsyncCall()
    } catch (error) {
        logger.error('Operation failed:', error)
        throw error
    }
}

// CORRECT: Top-level error handler
process.on('unhandledRejection', (reason, promise) => {
    logger.error('Unhandled Rejection:', reason)
    // Optionally exit process
    process.exit(1)
})
```

## Module System (CRITICAL)

ALWAYS use ES modules:

```javascript
// WRONG: CommonJS
const express = require('express')
module.exports = app

// CORRECT: ES modules
import express from 'express'
export default app

// CORRECT: package.json
{
    "type": "module",
    "main": "index.js"
}
```

## Environment Variables (MANDATORY)

ALWAYS use environment variables:

```javascript
// WRONG: Hardcoded config
const config = {
    port: 3000,
    dbUrl: 'mongodb://localhost:27017',
    apiKey: 'sk-1234567890'
}

// CORRECT: Environment variables
import 'dotenv/config'

const config = {
    port: process.env.PORT || 3000,
    dbUrl: process.env.DATABASE_URL,
    apiKey: process.env.API_KEY
}

if (!config.apiKey) {
    throw new Error('API_KEY environment variable is required')
}
```

## Streams (CRITICAL)

ALWAYS use streams for large data:

```javascript
// WRONG: Loading entire file into memory
import { readFileSync } from 'fs'

const data = readFileSync('large-file.txt')  // Memory issue!

// CORRECT: Streams
import { createReadStream } from 'fs'
import { pipeline } from 'stream/promises'

await pipeline(
    createReadStream('large-file.txt'),
    transformStream,
    createWriteStream('output.txt')
)
```

## TypeScript (MANDATORY)

ALWAYS use TypeScript:

```typescript
// CORRECT: TypeScript with Node.js
import express, { Request, Response, NextFunction } from 'express'

interface User {
    id: string
    name: string
    email: string
}

async function getUser(id: string): Promise<User | null> {
    // Implementation
}

app.get('/users/:id', async (req: Request, res: Response) => {
    const user = await getUser(req.params.id)
    if (!user) {
        return res.status(404).json({ error: 'User not found' })
    }
    res.json(user)
})
```

## Code Quality Checklist

Before marking work complete:
- [ ] Async/await used (no callbacks)
- [ ] Errors properly handled (try-catch)
- [ ] ES modules used (not CommonJS)
- [ ] Environment variables for config
- [ ] Streams for large data processing
- [ ] TypeScript enabled
- [ ] No console.log (use logger)
- [ ] Process exit handlers
- [ ] Graceful shutdown implemented
- [ ] ESLint + TypeScript rules passing
