---
name: django-bolt
description: "Django Bolt - Rust-powered high-performance API framework, 60k+ RPS, decorator routing, built-in auth, async ORM"
metadata:
  author: OSS AI Skills
  version: 1.0.0
  tags:
    - python
    - django
    - bolt
    - api
    - rust
    - performance
    - async
    - high-performance
---

# Django Bolt

High-performance fully typed API framework for Django — **Faster than FastAPI, but with Django ORM, Django Admin, and Django packages**.

Django-Bolt is a Rust-powered API framework achieving **60k+ RPS**. Uses Actix Web for HTTP, PyO3 for Python bridging, and msgspec for serialization.

## Overview

**Architecture:**
- **HTTP Server**: Actix Web (Rust) — one of the fastest HTTP frameworks
- **Python Bridge**: PyO3 — seamless Rust-Python integration  
- **Serialization**: msgspec — 5-10x faster than stdlib
- **Routing**: matchit — zero-copy path matching
- **Async Runtime**: Tokio

**Why Django Bolt?**
- No gunicorn or uvicorn needed — deploy directly
- Full Django ORM integration with async support
- Built-in auth (JWT, API Key) in Rust (no Python GIL)
- OpenAPI auto-generation
- Compatible with existing Django packages

---

## Installation

```bash
pip install django-bolt
```

```python
# settings.py
INSTALLED_APPS = [
    ...
    "django_bolt"
    ...
]
```

---

## Quick Start

### Basic API

```python
# api.py
from django_bolt import BoltAPI
from django.contrib.auth import get_user_model
import msgspec

User = get_user_model()

api = BoltAPI()

class UserSchema(msgspec.Struct):
    id: int
    username: str

@api.get("/users/{user_id}")
async def get_user(user_id: int) -> UserSchema:
    # Response is type validated
    user = await User.objects.aget(id=user_id)
    # Django ORM works without any setup
    return {"id": user.id, "username": user.username}
```

### Running the Server

```bash
# Development
python manage.py runbolt --dev

# Production (standalone)
python manage.py runbolt
```

---

## Routing

### HTTP Methods

```python
from django_bolt import BoltAPI

api = BoltAPI()

@api.get("/endpoint")
async def get_handler(request):
    return {"message": "GET"}

@api.post("/endpoint")
async def post_handler(request):
    data = await request.json()
    return {"received": data}

@api.put("/endpoint")
async def put_handler(request):
    return {"message": "PUT"}

@api.delete("/endpoint/{id}")
async def delete_handler(id: int):
    return {"deleted": id}

@api.patch("/endpoint")
async def patch_handler(request):
    return {"message": "PATCH"}
```

### Path Parameters

```python
@api.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"id": user_id}

@api.get("/posts/{post_id}/comments/{comment_id}")
async def get_comment(post_id: int, comment_id: int):
    return {"post_id": post_id, "comment_id": comment_id}
```

### Query Parameters

```python
from django_bolt import Query

@api.get("/search")
async def search_handler(
    query: str = Query(...),
    limit: int = Query(10),
    offset: int = Query(0)
):
    return {"query": query, "limit": limit, "offset": offset}
```

### Request Body

```python
import msgspec

class CreateUserRequest(msgspec.Struct):
    username: str
    email: str
    password: str

@api.post("/users")
async def create_user(request, body: CreateUserRequest):
    # body is automatically validated
    user = await User.objects.acreate(
        username=body.username,
        email=body.email
    )
    return {"id": user.id, "username": user.username}
```

---

## Authentication

### JWT Authentication

```python
from django_bolt.auth import JWTBearer, jwt_required
from django_bolt.response import Json

auth = JWTBearer(secret_key="your-secret-key")

@api.get("/protected", guards=[jwt_required])
async def protected_handler(request):
    return {"user": request.user.id}
```

### API Key Authentication

```python
from django_bolt.auth import APIKeyBearer, api_key_required

auth = APIKeyBearer()

@api.get("/api-protected", guards=[api_key_required])
async def api_protected_handler(request):
    return {"message": "API key authenticated"}
```

### Custom Authentication

```python
from django_bolt.auth import BaseAuth, AuthResult
from django.contrib.auth import get_user_model

User = get_user_model()

class CustomAuth(BaseAuth):
    async def authenticate(self, request) -> AuthResult:
        token = request.headers.get("Authorization")
        if token and token.startswith("Bearer "):
            # Validate token
            user = await self.get_user(token)
            return AuthResult(user=user)
        return AuthResult()

async def get_user(self, token: str):
    # Your logic to get user from token
    try:
        return await User.objects.aget(id=int(token.split("_")[1]))
    except:
        return None
```

---

## Permissions & Guards

### Built-in Guards

```python
from django_bolt.auth import IsAuthenticated, HasPermission, HasRole

# Require authentication
@api.get("/private", guards=[IsAuthenticated])
async def private_handler(request):
    return {"user_id": request.user.id}

# Require specific permission
@api.get("/edit-post", guards=[HasPermission("blog.change_post")])
async def edit_post_handler(request):
    return {"can_edit": True}

# Require role
@api.get("/admin-only", guards=[HasRole("admin")])
async def admin_handler(request):
    return {"access": "granted"}
```

### Custom Guards

```python
from django_bolt.auth import BaseGuard, AuthResult

class CustomGuard(BaseGuard):
    async def check(self, request) -> bool:
        # Your logic
        return request.headers.get("X-Custom-Header") == "secret"
```

---

## Middleware

### Built-in Middleware

```python
from django_bolt.middleware import CORSMiddleware, RateLimitMiddleware, CompressionMiddleware

api = BoltAPI(
    middleware=[
        CORSMiddleware(
            allow_origins=["*"],
            allow_methods=["*"],
            allow_headers=["*"],
        ),
        RateLimitMiddleware(requests=100, window=60),  # 100 requests per minute
        CompressionMiddleware(),
    ]
)
```

### Django Middleware Integration

```python
from django.middleware.security import SecurityMiddleware

api = BoltAPI(
    django_middleware=[
        SecurityMiddleware,
        # Your Django middleware here
    ]
)
```

---

## Responses

### JSON Response

```python
from django_bolt.response import Json

@api.get("/json")
async def json_handler(request):
    return Json({"key": "value"})

# Or shorthand (automatically JSON serialized)
@api.get("/auto-json")
async def auto_json_handler(request):
    return {"key": "value"}
```

### HTML Response

```python
from django_bolt.response import Html

@api.get("/html")
async def html_handler(request):
    return Html("<h1>Hello World</h1>")
```

### Streaming Response (SSE)

```python
from django_bolt.response import StreamingResponse

async def event_stream():
    for i in range(10):
        yield f"data: message {i}\n\n"
    # Or use Server-Sent Events format
    yield {"event": "message", "data": {"count": i}}

@api.get("/stream")
async def stream_handler(request):
    return StreamingResponse(event_stream())
```

### File Response

```python
from django_bolt.response import FileResponse

@api.get("/download")
async def download_handler(request):
    return FileResponse(
        path="/path/to/file.pdf",
        filename="document.pdf",
        content_type="application/pdf"
    )
```

### Redirect Response

```python
from django_bolt.response import Redirect

@api.get("/redirect")
async def redirect_handler(request):
    return Redirect(url="https://example.com", status=302)
```

---

## Class-Based Views

### ViewSet

```python
from django_bolt.views import ViewSet, route

class UserViewSet(ViewSet):
    @route.get("/users")
    async def list(self, request):
        users = await User.objects.alist()
        return {"users": [{"id": u.id, "username": u.username} for u in users]}

    @route.get("/users/{pk}")
    async def retrieve(self, request, pk: int):
        user = await User.objects.aget(id=pk)
        return {"id": user.id, "username": user.username}

    @route.post("/users")
    async def create(self, request):
        data = await request.json()
        user = await User.objects.acreate(**data)
        return {"id": user.id}

    @route.put("/users/{pk}")
    async def update(self, request, pk: int):
        data = await request.json()
        user = await User.objects.aget(id=pk)
        for k, v in data.items():
            setattr(user, k, v)
        await user.asave()
        return {"id": user.id}

    @route.delete("/users/{pk}")
    async def destroy(self, request, pk: int):
        user = await User.objects.aget(id=pk)
        await user.adelete()
        return {"deleted": True}

api.register_viewset(UserViewSet, prefix="/api")
```

### ModelViewSet

```python
from django_bolt.views import ModelViewSet
from django.contrib.auth import get_user_model
from django_bolt.serializers import ModelSerializer

User = get_user_model()

class UserSerializer(ModelSerializer):
    class Meta:
        model = User
        fields = ["id", "username", "email"]

class UserModelViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

api.register_viewset(UserModelViewSet, prefix="/api")
```

---

## Serializers (msgspec)

msgspec provides **5-10x faster** JSON serialization than Python's stdlib.

### Basic Struct

```python
import msgspec

class UserSchema(msgspec.Struct):
    id: int
    username: str
    email: str
    is_active: bool = True

@api.post("/users")
async def create_user(request, body: UserSchema):
    # body is automatically validated
    user = await User.objects.acreate(
        username=body.username,
        email=body.email,
        is_active=body.is_active
    )
    return {"id": user.id}
```

### With Validation

```python
import msgspec

class UserCreateSchema(msgspec.Struct):
    username: str
    email: str
    password: str
    
    def __post_init__(self):
        if len(self.password) < 8:
            raise msgspec.ValidationError("Password must be at least 8 characters")
        if "@" not in self.email:
            raise msgspec.ValidationError("Invalid email format")
```

### Nested Structures

```python
class AddressSchema(msgspec.Struct):
    street: str
    city: str
    zip_code: str
    country: str

class UserSchema(msgspec.Struct):
    id: int
    username: str
    address: AddressSchema | None = None

@api.get("/users/{user_id}")
async def get_user_with_address(user_id: int):
    user = await User.objects.aget(id=user_id)
    return {
        "id": user.id,
        "username": user.username,
        "address": {
            "street": user.street,
            "city": user.city,
            "zip_code": user.zip_code,
            "country": user.country
        } if user.street else None
    }
```

---

## OpenAPI / API Documentation

Django Bolt auto-generates OpenAPI documentation.

### Access Docs

- **Swagger**: `/docs`
- **ReDoc**: `/redoc`
- **Scalar**: `/scalar`
- **RapidDoc**: `/rapiddoc`

### Configure OpenAPI

```python
from django_bolt import BoltAPI
from django_bolt.openapi import OpenAPIInfo

api = BoltAPI(
    info=OpenAPIInfo(
        title="My API",
        version="1.0.0",
        description="API description",
    )
)
```

---

## Testing

### Test Client

```python
from django_bolt.test import AsyncAPITestClient

class UserAPITest(AsyncAPITestClient):
    async def test_create_user(self):
        response = await self.post(
            "/api/users",
            json={"username": "testuser", "email": "test@example.com"}
        )
        self.assertEqual(response.status_code, 201)
        data = await response.json()
        self.assertEqual(data["username"], "testuser")

    async def test_get_user(self):
        # Create user first
        user = await User.objects.acreate(username="testuser", email="test@example.com")
        
        response = await self.get(f"/api/users/{user.id}")
        self.assertEqual(response.status_code, 200)
```

---

## Performance Benchmarks

### Standard Endpoints

| Endpoint Type | Requests/sec |
|--------------|-------------|
| Root endpoint | **~100,000 RPS** |
| JSON parsing/validation (10kb) | **~83,700 RPS** |
| Path + Query parameters | **~85,300 RPS** |
| HTML response | **~100,600 RPS** |
| Redirect response | **~96,300 RPS** |
| Form data handling | **~76,800 RPS** |
| ORM reads (SQLite, 10 records) | **~13,000 RPS** |

### Streaming (SSE)

**10,000 concurrent clients:**
- Total Throughput: 9,489 messages/sec
- Successful Connections: 100%
- CPU Usage: 11.9% average

---

## Configuration

### Settings

```python
# settings.py

# Optional: Configure Bolt
BOLT = {
    "HOST": "0.0.0.0",
    "PORT": 8000,
    "DEBUG": False,
    "workers": 4,  # Number of worker processes
}

# Optional: JWT settings
JWT_SECRET_KEY = "your-secret-key"
JWT_ALGORITHM = "HS256"
JWT_EXPIRATION = 3600  # seconds
```

### Production Deployment

```bash
# Run in production mode
python manage.py runbolt --workers 4
```

---

## Why So Fast?

- **Actix Web**: Rust HTTP framework (one of the fastest)
- **matchit**: Zero-copy path matching
- **msgspec**: 5-10x faster JSON serialization
- **PyO3**: Direct Rust-Python interop
- **No GIL for auth**: JWT/API Key validation in Rust

---

## References

- **GitHub**: https://github.com/dj-bolt/django-bolt
- **Documentation**: https://bolt.farhana.li
- **Performance Comparison**: Faster than FastAPI, similar to Go/Node.js performance