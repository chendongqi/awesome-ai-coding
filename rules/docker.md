# Docker & Containerization Rules

## Dockerfile Best Practices (CRITICAL)

ALWAYS use multi-stage builds:

```dockerfile
# WRONG: Single stage with all tools
FROM node:18
RUN apt-get update && apt-get install -y \
    build-essential python3 git
COPY . .
RUN npm install
RUN npm run build
CMD ["node", "dist/index.js"]

# CORRECT: Multi-stage build
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Stage 2: Runtime
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
USER node
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

## Security (MANDATORY)

ALWAYS run as non-root user:

```dockerfile
# WRONG: Running as root
FROM node:18
COPY . .
RUN npm install
CMD ["node", "index.js"]

# CORRECT: Non-root user
FROM node:18-alpine
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001
WORKDIR /app
COPY --chown=nodejs:nodejs . .
USER nodejs
CMD ["node", "index.js"]
```

## Layer Caching (CRITICAL)

ALWAYS optimize layer caching:

```dockerfile
# WRONG: Copy everything first
FROM node:18
COPY . .
RUN npm install  # Cache invalidated on any file change

# CORRECT: Copy dependencies first
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .  # Only invalidates cache when code changes
```

## .dockerignore (MANDATORY)

ALWAYS use .dockerignore:

```dockerignore
# CORRECT: .dockerignore
node_modules
npm-debug.log
.git
.gitignore
.env
.env.local
dist
coverage
*.md
.DS_Store
```

## Health Checks (CRITICAL)

ALWAYS add health checks:

```dockerfile
# CORRECT: Health check
FROM node:18-alpine
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})"
```

## Docker Compose (MANDATORY)

ALWAYS use docker-compose for multi-container apps:

```yaml
# CORRECT: docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=${DATABASE_URL}
    depends_on:
      - db
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## Code Quality Checklist

Before marking work complete:
- [ ] Multi-stage builds used
- [ ] Non-root user configured
- [ ] Layer caching optimized
- [ ] .dockerignore file present
- [ ] Health checks added
- [ ] No hardcoded secrets
- [ ] Environment variables used
- [ ] Minimal base images (alpine)
- [ ] Proper WORKDIR set
- [ ] EXPOSE ports documented
