# HTTP (Hypertext Transfer Protocol)

HTTP is the foundational protocol for communication on the World Wide Web. It defines how messages are formatted and transmitted between web servers and clients (browsers).

---

## Table of Contents
- [Core Concepts](#core-concepts)
- [HTTP Methods](#http-methods)
- [HTTP Request Structure](#http-request-structure)
- [HTTP Response Structure](#http-response-structure)
- [HTTP Headers](#http-headers)
- [HTTP Status Codes](#http-status-codes)
- [HTTPS vs HTTP](#https-vs-http)
- [Working with HTTP](#working-with-http)
- [Practical Tools](#practical-tools)

---

## Core Concepts

### What is HTTP?

**Definition:** HTTP stands for **Hypertext Transfer Protocol**

**Purpose:**
- Foundational protocol for communication between web servers and clients
- Enables transfer of hypertext (text with links, images, etc.)
- Used by every website and web application

**How it works:**
```
Client (Browser) ─── HTTP Request ───→ Web Server
      ↑                                      │
      └───── HTTP Response ─────────────────┘
```

---

### Request/Response Cycle

Every HTTP interaction follows this pattern:

1. **Client sends HTTP Request**
   - Asks the server for something
   - Contains: URL, method, headers, body (optional)

2. **Server processes the request**
   - Validates the request
   - Retrieves or modifies data
   - Prepares a response

3. **Server sends HTTP Response**
   - Contains: status code, headers, body (optional)

4. **Client receives and processes response**
   - Displays content, handles errors, etc.

**Example:**
```
You type: www.example.com
Browser: GET / HTTP/1.1
Server: 200 OK, Here's the HTML
Browser: Renders the webpage
```

---

### Statelessness

**Definition:** HTTP is a **stateless** protocol

**What it means:**
- Every request is an **independent transaction**
- Server does NOT remember previous interactions
- Each request must contain ALL necessary information
- No built-in memory or session tracking

**Example:**
```
Request 1: User logs in → Server responds "OK"
Request 2: User asks for profile → Server doesn't know who they are!
```

**Solutions for maintaining state:**

| Method | How it works | Storage |
|--------|-------------|---------|
| **Cookies** | Small data sent with every request | Client browser |
| **Sessions** | Server stores user data, client has session ID | Server-side |
| **Tokens (JWT)** | Encrypted data in request headers | Client-side |
| **Local Storage** | Data stored in browser | Client browser |

---

## HTTP Methods

HTTP methods define **what action** you want to perform on a resource.

### The Big Four: CRUD Operations

**CRUD** = Create, Read, Update, Delete

| Method | CRUD Action | Purpose | Idempotent | Safe |
|--------|------------|---------|------------|------|
| **GET** | Read | Retrieve data | ✅ Yes | ✅ Yes |
| **POST** | Create | Submit/add new data | ❌ No | ❌ No |
| **PUT** | Update | Replace existing data | ✅ Yes | ❌ No |
| **DELETE** | Delete | Remove data | ✅ Yes | ❌ No |

**Idempotent:** Making the same request multiple times has the same effect as once
**Safe:** Does not modify server data

---

### GET Method

**Purpose:** Retrieve data from the server

**Characteristics:**
- ✅ **Safe** — does not modify data
- ✅ **Idempotent** — same result every time
- ❌ **Cannot** send request body (use URL parameters)
- 📏 **Cached** — browsers cache GET requests

**Example:**
```http
GET /users/123 HTTP/1.1
Host: api.example.com
```

**Use cases:**
- Loading web pages
- Fetching user profiles
- Retrieving search results
- Downloading files

**URL Parameters:**
```
GET /search?query=laptop&category=electronics
```

---

### POST Method

**Purpose:** Submit/add new data to the server

**Characteristics:**
- ❌ **Not safe** — modifies data
- ❌ **Not idempotent** — multiple requests create multiple resources
- ✅ **Can** send request body
- 📦 **Not cached** — always processed by server

**Example:**
```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Use cases:**
- Creating user accounts
- Submitting forms
- Uploading files
- Sending messages

---

### PUT Method

**Purpose:** Update/replace existing data on the server

**Characteristics:**
- ❌ **Not safe** — modifies data
- ✅ **Idempotent** — same result every time
- ✅ **Can** send request body
- 🔄 **Complete replacement** — sends entire resource

**Example:**
```http
PUT /users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "id": 123,
  "name": "John Updated",
  "email": "newemail@example.com"
}
```

**Use cases:**
- Updating user profiles
- Modifying settings
- Replacing entire records

**Note:** There's also **PATCH** for partial updates (only send changed fields)

---

### DELETE Method

**Purpose:** Remove data from the server

**Characteristics:**
- ❌ **Not safe** — modifies data
- ✅ **Idempotent** — deleting twice still results in deleted resource
- ⚠️ **May or may not** have request body
- 🗑️ **Permanent** — data is removed

**Example:**
```http
DELETE /users/123 HTTP/1.1
Host: api.example.com
```

**Use cases:**
- Deleting user accounts
- Removing posts/comments
- Canceling orders

---

### Other HTTP Methods

| Method | Purpose | Usage |
|--------|---------|-------|
| **PATCH** | Partial update | Update specific fields |
| **HEAD** | Get headers only | Check if resource exists |
| **OPTIONS** | Get supported methods | CORS preflight requests |
| **CONNECT** | Establish tunnel | Proxy connections |
| **TRACE** | Diagnostic | Loop-back test (rarely used) |

---

## HTTP Request Structure

An HTTP request has **three main parts**:

```
┌─────────────────────────────────────────┐
│ Request Line                            │
│ GET /users/123 HTTP/1.1                 │
├─────────────────────────────────────────┤
│ Headers                                 │
│ Host: api.example.com                   │
│ User-Agent: Mozilla/5.0                 │
│ Accept: application/json                │
│ Authorization: Bearer token123          │
├─────────────────────────────────────────┤
│ Body (optional)                         │
│ {                                       │
│   "name": "John",                       │
│   "email": "john@example.com"           │
│ }                                       │
└─────────────────────────────────────────┘
```

### 1. Request Line

First line of the request, contains:
- **Method** — what to do (GET, POST, PUT, DELETE)
- **URI** — what resource (path and query parameters)
- **HTTP Version** — which HTTP version (HTTP/1.1, HTTP/2)

**Example:**
```
GET /api/users?page=1&limit=10 HTTP/1.1
```

### 2. Headers

Key-value pairs providing **metadata** about the request:

```http
Host: api.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Length: 45
```

### 3. Body (Optional)

Data sent to the server (used with POST, PUT, PATCH):

**JSON format:**
```json
{
  "username": "johndoe",
  "password": "secret123"
}
```

**Form data:**
```
username=johndoe&password=secret123
```

**Note:** GET requests typically do NOT have a body

---

## HTTP Response Structure

An HTTP response has **three main parts**:

```
┌─────────────────────────────────────────┐
│ Status Line                             │
│ HTTP/1.1 200 OK                         │
├─────────────────────────────────────────┤
│ Headers                                 │
│ Content-Type: application/json          │
│ Content-Length: 256                     │
│ Set-Cookie: sessionId=abc123            │
├─────────────────────────────────────────┤
│ Body                                    │
│ {                                       │
│   "id": 123,                            │
│   "name": "John Doe",                   │
│   "email": "john@example.com"           │
│ }                                       │
└─────────────────────────────────────────┘
```

### 1. Status Line

First line of the response, contains:
- **HTTP Version** — which HTTP version was used
- **Status Code** — numeric result (200, 404, 500)
- **Reason Phrase** — human-readable description

**Example:**
```
HTTP/1.1 200 OK
HTTP/1.1 404 Not Found
HTTP/1.1 500 Internal Server Error
```

### 2. Headers

Key-value pairs providing **metadata** about the response:

```http
Content-Type: application/json
Content-Length: 256
Server: Apache/2.4.41
Date: Mon, 01 Jan 2024 12:00:00 GMT
Cache-Control: max-age=3600
Set-Cookie: sessionId=abc123; Path=/; HttpOnly
```

### 3. Body

The actual content returned (can be empty for some responses):

**Examples:**
- HTML document (web page)
- JSON data (API response)
- Image file
- Error message

---

## HTTP Headers

Headers provide **context and metadata** for requests and responses.

### Common Request Headers

| Header | Purpose | Example |
|--------|---------|---------|
| `Host` | Target domain | `api.example.com` |
| `User-Agent` | Client software | `Mozilla/5.0 (Windows NT 10.0)` |
| `Accept` | Preferred response format | `application/json` |
| `Content-Type` | Format of request body | `application/json` |
| `Authorization` | Authentication credentials | `Bearer token123` |
| `Cookie` | Stored cookies | `sessionId=abc123` |
| `Referer` | Previous page URL | `https://google.com` |
| `Content-Length` | Size of request body | `256` (bytes) |

### Common Response Headers

| Header | Purpose | Example |
|--------|---------|---------|
| `Content-Type` | Format of response body | `text/html; charset=utf-8` |
| `Content-Length` | Size of response body | `1024` (bytes) |
| `Server` | Server software | `nginx/1.18.0` |
| `Date` | Server timestamp | `Mon, 01 Jan 2024 12:00:00 GMT` |
| `Cache-Control` | Caching instructions | `max-age=3600, public` |
| `Set-Cookie` | Set a new cookie | `sessionId=abc123; Path=/` |
| `Location` | Redirect URL | `/login` or `https://example.com` |
| `WWW-Authenticate` | Auth method required | `Basic realm="Secure"` |

### Content-Type Examples

```http
Content-Type: text/html
Content-Type: application/json
Content-Type: text/plain
Content-Type: image/png
Content-Type: application/x-www-form-urlencoded
Content-Type: multipart/form-data
```

---

## HTTP Status Codes

Status codes inform the client about the **request outcome**.

### Status Code Categories

| Range | Category | Meaning |
|-------|----------|---------|
| **1xx** | Informational | Request received, processing continues |
| **2xx** | Success | Request successful |
| **3xx** | Redirection | Further action needed |
| **4xx** | Client Error | Bad request from client |
| **5xx** | Server Error | Server failed to fulfill request |

---

### 2xx Success Codes

| Code | Name | Description |
|------|------|-------------|
| **200** | OK | Request successful |
| **201** | Created | Resource successfully created (POST) |
| **202** | Accepted | Request accepted for processing (async) |
| **204** | No Content | Success, but no body returned |

**When to use:**
- `200` — Successful GET, PUT, DELETE
- `201` — Successful POST (resource created)
- `204` — Successful DELETE (no content to return)

---

### 3xx Redirection Codes

| Code | Name | Description |
|------|------|-------------|
| **301** | Moved Permanently | Resource has new permanent URL |
| **302** | Found | Resource temporarily at different URL |
| **304** | Not Modified | Resource unchanged (use cached version) |
| **307** | Temporary Redirect | Temporary redirect, keep method |

**When to use:**
- `301` — Domain moved, URL structure changed
- `302` — Temporary maintenance, A/B testing
- `304` — Browser cache validation

---

### 4xx Client Error Codes

| Code | Name | Description |
|------|------|-------------|
| **400** | Bad Request | Invalid syntax or parameters |
| **401** | Unauthorized | Authentication required |
| **403** | Forbidden | Valid credentials, but no permission |
| **404** | Not Found | Resource does not exist |
| **405** | Method Not Allowed | HTTP method not supported |
| **409** | Conflict | Resource conflict (e.g., duplicate email) |
| **422** | Unprocessable Entity | Valid syntax, but semantic errors |
| **429** | Too Many Requests | Rate limit exceeded |

**Common scenarios:**
- `400` — Missing required fields, invalid data format
- `401` — Not logged in, invalid token
- `403` — Logged in, but wrong role (e.g., user trying admin action)
- `404` — Wrong URL, deleted resource

---

### 5xx Server Error Codes

| Code | Name | Description |
|------|------|-------------|
| **500** | Internal Server Error | Unexpected server error |
| **502** | Bad Gateway | Invalid response from upstream server |
| **503** | Service Unavailable | Server overloaded or down for maintenance |
| **504** | Gateway Timeout | Upstream server didn't respond in time |

**When you see these:**
- Server-side bug (not client's fault)
- Check server logs for details
- May be temporary (retry) or permanent (needs fix)

---

## HTTPS vs HTTP

### What is HTTPS?

**HTTPS** = HTTP + **SSL/TLS** encryption

**Full name:** Hypertext Transfer Protocol **Secure**

### Key Differences

| Feature | HTTP | HTTPS |
|---------|------|-------|
| **Port** | 80 | 443 |
| **Encryption** | ❌ None | ✅ SSL/TLS |
| **Certificate** | ❌ Not required | ✅ Required |
| **Security** | ❌ Data in plaintext | ✅ Data encrypted |
| **SEO** | ❌ Lower ranking | ✅ Higher ranking |
| **Trust** | ❌ "Not Secure" warning | ✅ Padlock icon |

### How HTTPS Works

```
1. Client connects to server
2. Server sends SSL/TLS certificate
3. Client verifies certificate
4. Client and server negotiate encryption keys
5. All data encrypted in transit
```

### Why HTTPS is Important

✅ **Confidentiality** — Data cannot be read by third parties
✅ **Integrity** — Data cannot be tampered with
✅ **Authentication** — Server identity verified
✅ **Trust** — Browser shows padlock icon
✅ **SEO** — Google ranks HTTPS sites higher

**Modern Standard:**
- HTTPS is now the default for most websites
- Browsers mark HTTP as "Not Secure"
- Uses TLS 1.2 or 1.3 (SSL is deprecated)

---

## Working with HTTP

### Example: RESTful API Design

**Resource:** Users

| Action | Method | Endpoint | Request Body | Response |
|--------|--------|----------|--------------|----------|
| Get all users | `GET` | `/users` | None | `200` + array of users |
| Get one user | `GET` | `/users/123` | None | `200` + user object |
| Create user | `POST` | `/users` | User data | `201` + created user |
| Update user | `PUT` | `/users/123` | Complete user data | `200` + updated user |
| Partial update | `PATCH` | `/users/123` | Changed fields only | `200` + updated user |
| Delete user | `DELETE` | `/users/123` | None | `204` or `200` |

### Example Request/Response

**Request:**
```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer token123

{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "age": 28
}
```

**Response:**
```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/456

{
  "id": 456,
  "name": "Jane Doe",
  "email": "jane@example.com",
  "age": 28,
  "createdAt": "2024-01-01T12:00:00Z"
}
```

---

## Practical Tools

### 1. Browser DevTools

**Network Tab:** Inspect all HTTP activity

**What you can see:**
- Request URL and method
- Request headers and body
- Response status and headers
- Response body
- Timing information
- Cookies being set

**How to use:**
1. Open DevTools (F12 or Ctrl+Shift+I)
2. Go to Network tab
3. Refresh page or perform action
4. Click on request to see details

---

### 2. Postman

**Purpose:** GUI tool for testing APIs

**Features:**
- Send any HTTP method (GET, POST, PUT, DELETE, etc.)
- Set custom headers
- Send JSON, form data, or raw body
- Save requests for reuse
- Organize into collections
- Automated testing with scripts

**Example workflow:**
1. Create request: `POST http://api.example.com/users`
2. Set headers: `Content-Type: application/json`
3. Add body: `{"name": "John"}`
4. Click Send
5. View response (status, headers, body)

---

### 3. cURL (Command Line)

**Purpose:** Make HTTP requests from terminal

**Examples:**

**GET request:**
```bash
curl http://api.example.com/users
```

**GET with headers:**
```bash
curl -H "Authorization: Bearer token123" http://api.example.com/users/123
```

**POST request with JSON:**
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}' \
  http://api.example.com/users
```

**PUT request:**
```bash
curl -X PUT \
  -H "Content-Type: application/json" \
  -d '{"name":"John Updated"}' \
  http://api.example.com/users/123
```

**DELETE request:**
```bash
curl -X DELETE http://api.example.com/users/123
```

**Verbose output (see headers):**
```bash
curl -v http://api.example.com/users
```

---

### 4. wget (Command Line)

**Purpose:** Download files and make requests

**Examples:**

**Download file:**
```bash
wget http://example.com/file.zip
```

**GET request:**
```bash
wget -qO- http://api.example.com/users
```

**POST request:**
```bash
wget --post-data='{"name":"John"}' \
  --header='Content-Type: application/json' \
  http://api.example.com/users
```

---

### 5. HTTPie (Modern CLI Tool)

**Purpose:** User-friendly HTTP client

**Installation:**
```bash
sudo apt install httpie  # Ubuntu/Debian
brew install httpie      # macOS
```

**Examples:**

**GET request:**
```bash
http GET http://api.example.com/users
```

**POST request:**
```bash
http POST http://api.example.com/users \
  name="John Doe" \
  email="john@example.com"
```

**With JSON body:**
```bash
echo '{"name":"John"}' | http POST http://api.example.com/users
```

---

## Summary

**Key takeaways:**

1. **HTTP** is the foundation of web communication
2. **Stateless** — every request is independent
3. **Methods** define actions (GET, POST, PUT, DELETE)
4. **Headers** provide metadata and context
5. **Status codes** indicate success or failure
6. **HTTPS** adds encryption for security
7. **Tools** like Postman, cURL, and DevTools help test APIs

---

## Related Topics

- [Protocols](protocols.md) - Overview of all networking protocols
- [HTTPS](protocols.md#https-http-secure) - Secure HTTP explained
- [OSI Model](osi-model.md) - Where HTTP fits (Layer 7: Application)
- [DNS](protocols.md#dns-domain-name-system) - How domain names are resolved
