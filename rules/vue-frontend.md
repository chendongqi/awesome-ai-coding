# Vue.js Frontend Development Rules

## Component Structure (CRITICAL)

ALWAYS use Composition API (Vue 3):

```vue
<!-- WRONG: Options API (Vue 2 style) -->
<script>
export default {
    data() {
        return {
            count: 0
        }
    },
    methods: {
        increment() {
            this.count++
        }
    }
}
</script>

<!-- CORRECT: Composition API -->
<script setup lang="ts">
import { ref, computed } from 'vue'

const count = ref(0)
const doubleCount = computed(() => count.value * 2)

function increment() {
    count.value++
}
</script>

<template>
    <div>
        <p>Count: {{ count }}</p>
        <p>Double: {{ doubleCount }}</p>
        <button @click="increment">Increment</button>
    </div>
</template>
```

## Reactivity (MANDATORY)

ALWAYS understand reactivity:

```typescript
// WRONG: Mutating non-reactive object
const state = { count: 0 }
state.count++  // Not reactive!

// CORRECT: Using ref
import { ref } from 'vue'

const count = ref(0)
count.value++  // Reactive!

// CORRECT: Using reactive
import { reactive } from 'vue'

const state = reactive({ count: 0 })
state.count++  // Reactive!
```

## Props & Emits (CRITICAL)

ALWAYS define props and emits:

```vue
<script setup lang="ts">
// CORRECT: Props with TypeScript
interface Props {
    title: string
    count?: number
}

const props = withDefaults(defineProps<Props>(), {
    count: 0
})

// CORRECT: Emits
interface Emits {
    (e: 'update', value: number): void
    (e: 'delete', id: string): void
}

const emit = defineEmits<Emits>()

function handleClick() {
    emit('update', props.count + 1)
}
</script>
```

## Composables (MANDATORY)

ALWAYS extract reusable logic into composables:

```typescript
// CORRECT: Custom composable
// composables/useUser.ts
import { ref, computed } from 'vue'
import type { User } from '@/types'

export function useUser(userId: string) {
    const user = ref<User | null>(null)
    const loading = ref(false)
    const error = ref<Error | null>(null)
    
    const isAdmin = computed(() => user.value?.role === 'admin')
    
    async function fetchUser() {
        loading.value = true
        error.value = null
        try {
            user.value = await userService.getUser(userId)
        } catch (e) {
            error.value = e as Error
        } finally {
            loading.value = false
        }
    }
    
    return {
        user,
        loading,
        error,
        isAdmin,
        fetchUser
    }
}

// Usage in component
<script setup lang="ts">
const { user, loading, fetchUser } = useUser('123')
</script>
```

## State Management (CRITICAL)

ALWAYS use Pinia for state management:

```typescript
// CORRECT: Pinia store
// stores/user.ts
import { defineStore } from 'pinia'
import type { User } from '@/types'

export const useUserStore = defineStore('user', {
    state: () => ({
        currentUser: null as User | null,
        users: [] as User[]
    }),
    
    getters: {
        isAuthenticated: (state) => state.currentUser !== null,
        userName: (state) => state.currentUser?.name ?? 'Guest'
    },
    
    actions: {
        async fetchUser(id: string) {
            this.currentUser = await userService.getUser(id)
        },
        
        async fetchUsers() {
            this.users = await userService.getUsers()
        }
    }
})

// Usage in component
<script setup lang="ts">
import { useUserStore } from '@/stores/user'

const userStore = useUserStore()
await userStore.fetchUser('123')
</script>
```

## TypeScript (CRITICAL)

ALWAYS use TypeScript:

```vue
<script setup lang="ts">
// CORRECT: TypeScript interfaces
interface User {
    id: string
    name: string
    email: string
}

const user = ref<User | null>(null)

// CORRECT: Typed composables
function useApi<T>(url: string) {
    const data = ref<T | null>(null)
    const loading = ref(false)
    
    async function fetch() {
        loading.value = true
        const response = await fetch(url)
        data.value = await response.json() as T
        loading.value = false
    }
    
    return { data, loading, fetch }
}
</script>
```

## Code Quality Checklist

Before marking work complete:
- [ ] Composition API used (not Options API)
- [ ] Props and emits properly typed
- [ ] Composables for reusable logic
- [ ] Pinia for state management
- [ ] TypeScript strict mode enabled
- [ ] No direct DOM manipulation
- [ ] Proper error handling
- [ ] Loading states handled
- [ ] Accessibility (a11y) attributes
- [ ] ESLint + Vue rules passing
