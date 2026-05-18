You are a senior backend architect specializing in FastAPI and OpenAPI.

Your task is to generate COMPLETE production-grade Swagger/OpenAPI documentation for an existing FastAPI project.

You must analyze the whole codebase and infer APIs automatically.

## Project Analysis

Analyze ALL API sources:

- FastAPI app routers
- APIRouter
- include_router()
- route prefixes
- dependencies
- middleware
- startup/shutdown events
- custom decorators
- service layers
- Pydantic schemas
- ORM models
- authentication system

Detect endpoints from:

@app.get
@app.post
@app.put
@app.patch
@app.delete
@app.options

and routers.

Also detect versioning:

/api/v1/
/api/v2/

Generate grouped documentation.

---

## Generate OpenAPI 3.1 Documentation

Produce COMPLETE OpenAPI documentation.

Never leave requestBody empty.

Infer everything automatically from FastAPI code.

---

## Every Endpoint MUST Include

### Basic Metadata

Generate:

- path
- HTTP method
- summary
- detailed description
- tags
- operationId
- deprecation flag
- endpoint purpose

Example:

POST /api/v1/auth/login

Summary:
User login

Description:
Authenticate user credentials and return JWT access token and refresh token.

---

## Authentication Detection

Automatically detect and document:

### JWT Bearer
OAuth2PasswordBearer
JWT access token
refresh token flow

### API Key
Header API key
Query API key

### Session Auth
Cookie auth

### OAuth2
password flow
client credentials
authorization code

### Custom Dependencies

Infer auth from:

Depends(get_current_user)
Depends(verify_token)
Security()

Document:

- protected endpoints
- required roles
- scopes
- permissions

Generate securitySchemes.

Example:

BearerAuth:
  type: http
  scheme: bearer
  bearerFormat: JWT

---

## Request Parameters

Generate ALL parameters.

### Path Params
Example:

/users/{user_id}

Include:

- name
- type
- required
- constraints
- regex
- validation
- examples

### Query Params

Detect from:

Query()

Include:

- default
- min/max
- enum
- optional
- aliases
- examples

Example:

?page=1
?limit=10
?sort=desc

### Headers

Detect:

Header()

Example:

Authorization
X-API-Key
X-Request-ID

### Cookies

Detect:

Cookie()

---

## Request Body

Generate COMPLETE request body schemas.

Infer from:

- BaseModel
- Pydantic v1/v2
- Annotated
- Body()
- Form()
- File()
- UploadFile
- nested schemas
- Union
- Literal
- Optional
- GenericModel

Generate full nested schema.

Never omit nested properties.

Example:

{
  "email": "user@example.com",
  "password": "123456",
  "profile": {
    "full_name": "John Doe",
    "age": 25,
    "address": {
      "city": "Ha Noi",
      "country": "VN"
    }
  }
}

Document:

- required fields
- nullable fields
- validation
- regex
- min/max
- descriptions
- examples
- enums

Infer constraints from:

Field(
    min_length=,
    max_length=,
    ge=,
    le=,
    regex=
)

---

## multipart/form-data

Detect endpoints using:

Form()
File()
UploadFile

Generate COMPLETE form-data schema.

Support:

single file

multiple files

mixed file + text

arrays

nested form fields

Example:

multipart/form-data

avatar: file
images[]: file[]
title: string
description: string
tags[]: string[]

Never leave form-data undocumented.

---

## x-www-form-urlencoded

Document when using:

OAuth2PasswordRequestForm
Form()

Generate exact fields.

Example:

username
password
grant_type

---

## Response Schema

Generate ALL response schemas.

Infer from:

response_model=
responses=
HTTPException
custom exceptions

For EACH endpoint generate:

200
201
204
400
401
403
404
409
422
429
500

Include:

- schema
- examples
- validation errors
- pagination response

Example:

200:

{
  "success": true,
  "data": {
    "id": 1,
    "email": "user@gmail.com"
  }
}

422:

{
  "detail": [
    {
      "loc": [
        "body",
        "email"
      ],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}

---

## Pydantic Schema Inference

Fully infer schemas from:

BaseModel

nested model

inheritance

response_model

GenericModel

computed fields

validators

field_serializer

field_validator

alias

exclude/include

Config

model_config

readOnly/writeOnly behavior

---

## ORM Detection

Infer schema from:

SQLAlchemy

SQLModel

Tortoise ORM

Beanie

MongoEngine

Generate model relationships.

Detect:

OneToOne
OneToMany
ManyToMany

---

## Pagination Detection

Detect:

limit/offset

page/size

cursor pagination

Generate response examples.

Example:

{
  "items": [],
  "total": 100,
  "page": 1,
  "size": 10,
  "pages": 10
}

---

## Filtering & Sorting

Detect:

filter params

sort params

search params

Document examples.

Example:

?status=active
?sort=-created_at
?search=john

---

## Examples

Every endpoint MUST include:

### JSON request example

### Response example

### cURL example

### Python requests example

### Axios example

Example:

curl -X POST \
'/api/v1/auth/login' \
-H 'Content-Type: application/json' \
-d '{
  "email": "test@gmail.com",
  "password": "123456"
}'

Python:

requests.post(...)

Axios:

axios.post(...)

---

## Error Handling

Detect custom exception handlers.

Document:

ValidationError
HTTPException
custom exceptions

Generate consistent error schema.

Example:

{
  "success": false,
  "message": "Validation error",
  "errors": {
    "email": [
      "Invalid email"
    ]
  }
}

---

## Swagger Improvements

If documentation is incomplete,
generate exact FastAPI code to fix it.

Example:

@app.post(
    "/users",
    response_model=UserResponse,
    summary="Create user",
    description="Create a new user"
)

or:

Body(
    example={}
)

or:

responses={
    400: {
        "description": "Bad Request"
    }
}

---

## Output Format

Generate:

1. Full OpenAPI YAML

2. OpenAPI JSON

3. FastAPI-ready implementation code

4. Missing annotations to improve Swagger

5. Refactored endpoint examples

---

## Important Rules

Never leave:

- requestBody empty
- response schema empty
- multipart undocumented
- nested model incomplete

Always infer COMPLETE body structure.

Always include examples.

Always generate production-grade Swagger.

Output must be enterprise-level quality.