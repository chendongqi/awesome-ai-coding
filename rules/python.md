# Python Development Rules

## Type Hints (CRITICAL)

ALWAYS use type hints:

```python
# WRONG: No type hints
def process_user(data):
    return data['name']

# CORRECT: Type hints
from typing import Dict, List, Optional

def process_user(data: Dict[str, any]) -> str:
    return data['name']

# CORRECT: Modern type hints (Python 3.9+)
def get_users(limit: int = 10) -> list[dict[str, any]]:
    return []

# CORRECT: Optional and Union
from typing import Optional, Union

def find_user(user_id: str) -> Optional[dict[str, any]]:
    return None

def process_value(value: Union[int, str]) -> str:
    return str(value)
```

## Error Handling (MANDATORY)

ALWAYS use specific exceptions:

```python
# WRONG: Bare except
try:
    result = risky_operation()
except:
    pass  # Catches everything!

# CORRECT: Specific exceptions
try:
    result = risky_operation()
except ValueError as e:
    logger.error(f"Invalid value: {e}")
    raise
except FileNotFoundError as e:
    logger.error(f"File not found: {e}")
    raise
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    raise
```

## Code Style (CRITICAL)

ALWAYS follow PEP 8:

```python
# WRONG: Inconsistent naming
def getUserData():
    user_name = "John"
    UserAge = 25
    return {"name": user_name, "age": UserAge}

# CORRECT: PEP 8 compliant
def get_user_data() -> dict[str, any]:
    user_name = "John"
    user_age = 25
    return {"name": user_name, "age": user_age}

# CORRECT: Class naming
class UserService:
    def __init__(self, db: Database) -> None:
        self.db = db
```

## List Comprehensions (MANDATORY)

PREFER list comprehensions over loops:

```python
# WRONG: Verbose loop
result = []
for item in items:
    if item.is_valid():
        result.append(item.process())

# CORRECT: List comprehension
result = [item.process() for item in items if item.is_valid()]
```

## Context Managers (CRITICAL)

ALWAYS use context managers for resources:

```python
# WRONG: Manual resource management
file = open('data.txt', 'r')
data = file.read()
file.close()  # Easy to forget!

# CORRECT: Context manager
with open('data.txt', 'r') as file:
    data = file.read()

# CORRECT: Custom context manager
from contextlib import contextmanager

@contextmanager
def database_connection():
    conn = create_connection()
    try:
        yield conn
    finally:
        conn.close()

with database_connection() as conn:
    result = conn.execute(query)
```

## Async/Await (MANDATORY)

ALWAYS use async/await for I/O operations:

```python
# WRONG: Blocking I/O
def fetch_data(url: str) -> dict:
    response = requests.get(url)  # Blocks!
    return response.json()

# CORRECT: Async I/O
import aiohttp
import asyncio

async def fetch_data(url: str) -> dict:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()
```

## Data Validation (CRITICAL)

ALWAYS validate data with Pydantic:

```python
# WRONG: Manual validation
def create_user(name: str, email: str, age: int):
    if not name or len(name) > 100:
        raise ValueError("Invalid name")
    if '@' not in email:
        raise ValueError("Invalid email")
    # ... more validation

# CORRECT: Pydantic models
from pydantic import BaseModel, EmailStr, Field, validator

class UserCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    email: EmailStr
    age: int = Field(..., ge=0, le=150)
    
    @validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v.strip()

def create_user(user_data: UserCreate) -> User:
    # Validation already done by Pydantic
    return user_service.create(user_data.dict())
```

## Dependency Injection (MANDATORY)

PREFER dependency injection:

```python
# WRONG: Hard dependencies
class UserService:
    def __init__(self):
        self.db = Database()  # Hard dependency!
        self.cache = RedisCache()

# CORRECT: Dependency injection
class UserService:
    def __init__(self, db: Database, cache: Cache):
        self.db = db
        self.cache = cache

# Usage
db = Database()
cache = RedisCache()
user_service = UserService(db, cache)
```

## Testing (CRITICAL)

ALWAYS write tests:

```python
# CORRECT: pytest tests
import pytest
from unittest.mock import Mock, patch

def test_get_user_success():
    # Arrange
    mock_db = Mock()
    mock_db.get_user.return_value = {"id": "1", "name": "John"}
    service = UserService(mock_db)
    
    # Act
    result = service.get_user("1")
    
    # Assert
    assert result["name"] == "John"
    mock_db.get_user.assert_called_once_with("1")

# CORRECT: Fixtures
@pytest.fixture
def user_service():
    db = Mock()
    return UserService(db)

def test_get_user(user_service):
    result = user_service.get_user("1")
    assert result is not None
```

## Code Quality Checklist

Before marking work complete:
- [ ] Type hints on all functions
- [ ] PEP 8 compliant code style
- [ ] Specific exception handling
- [ ] Context managers for resources
- [ ] Async/await for I/O operations
- [ ] Pydantic models for validation
- [ ] Dependency injection used
- [ ] Unit tests written (pytest)
- [ ] No print() statements (use logging)
- [ ] No hardcoded values
- [ ] Docstrings for public APIs
