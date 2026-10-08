# HTTP Cookies

A **small piece of data stored in our computer/browser.**
Cookies are created/stored when you receive a `Set-Cookie` header from a web server.
Then, for future requests, the browser can send the cookie back to the server.
This allows the web server to **recognize you / maintain your session**.

---

## Cookie — First-Time Login

### First-time login

```text
        User
          │
          │  POST + credentials
          │
          ▼
   ┌───────────────┐
   │   Web Server  │
   └───────────────┘
          │
          │  Response
          │  Set-Cookie:
          │  session-id=xyz123
          ▼
        Browser
          │
          │
          ▼
     Cookie stored
     in browser
```

### Flow

```text
1. User logs in for the first time.

2. Browser sends a POST request
   along with credentials.

3. Server verifies the login.

4. Server sends a response containing:

   Set-Cookie: session-id=xyz123

5. Browser stores the cookie for later requests.
```

---

## Cookie — Later Requests

Once the browser has the cookie, it sends it with later requests.

```text
        User / Browser
              │
              │  Request
              │  Cookie:
              │  session-id=xyz123
              │
              ▼
       ┌───────────────┐
       │   Web Server  │
       └───────────────┘
              │
              │
              │ Check session
              │
              ▼
        Same user/session
              │
              │
              ▼
           Response
```

### Flow

```text
1. Browser sends a request with:

   Cookie: session-id=xyz123

2. Server checks the session.

3. Server knows this is the same user/session.

4. Server sends the response normally.
```

---

## Simple Understanding — Login + Cookie

```text
FIRST-TIME LOGIN
────────────────────────────────────

1. User fills login form

              │
              ▼

2. Browser sends POST request
   with credentials

              │
              ▼

3. Server verifies login

              │
              ▼

4. Server sends response:

   Set-Cookie: session-id=xyz123

              │
              ▼

5. Browser stores the cookie
   for later requests
```

### Later Request

```text
1. Browser sends request with:

   Cookie: session-id=xyz123

              │
              ▼

2. Server checks the session

              │
              ▼

3. Server knows this is
   the same user

              │
              ▼

4. Response is sent normally
```

---

## Full Cookie Flow Diagram

### New User

```text
┌──────────┐                         ┌─────────────┐
│   User   │ ─────── Request ──────►│ Web Server  │
└──────────┘                         └─────────────┘
      ▲                                     │
      │                                     │
      └─────────── Response ────────────────┘
```

---

### User Logs In

```text
┌──────────┐                         ┌─────────────┐
│   User   │                         │ Web Server  │
│          │                         │             │
│ Login    │                         │             │
│ Details  │                         │             │
└────┬─────┘                         └──────┬──────┘
     │                                      │
     │ POST request                         │
     │ + credentials                        │
     ├─────────────────────────────────────►│
     │                                      │
     │                                      │ Verify
     │                                      │ credentials
     │                                      │
     │ Response                             │
     │ Set-Cookie:                          │
     │ session-id=xyz123                    │
     │◄─────────────────────────────────────┤
     │                                      │
     ▼
Browser stores cookie
```

---

### Later Request

```text
┌──────────┐                         ┌─────────────┐
│   User   │                         │ Web Server  │
│          │                         │             │
│ Opens    │                         │             │
│ website  │                         │             │
└────┬─────┘                         └──────┬──────┘
     │                                      │
     │ Request                              │
     │ Cookie: session-id=xyz123            │
     ├─────────────────────────────────────►│
     │                                      │
     │                                      │ Check
     │                                      │ session
     │                                      │
     │ Response                             │
     │◄─────────────────────────────────────┤
     │                                      │
```

---

## Cookie Concept in One Picture

```text
             FIRST LOGIN

Browser ────── credentials ──────► Server
Browser ◄──── Set-Cookie ───────── Server
                  │
                  │
                  ▼
          session-id=xyz123
             stored in
              browser


             LATER REQUEST

Browser ─── Cookie: session-id=xyz123 ───► Server
                                             │
                                             ▼
                                      Check session
                                             │
                                             ▼
                                      Recognize user
                                             │
                                             ▼
Browser ◄────────── Response ────────────────┘
```

## The important idea

```text
Set-Cookie
    ↓
Browser stores cookie
    ↓
Browser sends cookie on later requests
    ↓
Server checks session ID
    ↓
Server recognizes the session
```

---

# Example HTTP Exchange

A simplified example of what this looks like at the HTTP level:

### 1. Login Request

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=junaid&password=example-password
```

### 2. Server Response

```http
HTTP/1.1 200 OK
Set-Cookie: session-id=xyz123
Content-Type: text/html
```

The browser receives the `Set-Cookie` header and stores the cookie.

### 3. Later Request

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session-id=xyz123
```

The server can use `session-id=xyz123` to look up the corresponding session.

---

# Important Cookie Headers

Cookies can contain additional attributes that control how browsers handle them.

For example:

```http
Set-Cookie: session-id=xyz123; Secure; HttpOnly; SameSite=Lax
```

### `Secure`

The cookie should only be sent over HTTPS.

### `HttpOnly`

JavaScript running in the browser cannot directly access the cookie through `document.cookie`.

### `SameSite`

Controls when the browser sends the cookie in cross-site requests and helps reduce certain cross-site request risks.

---

# Important Security Concept

A session cookie can act as a **bearer credential**.

For example:

```text
session-id=xyz123
```

If an attacker obtains a valid session cookie, they may potentially be able to make requests as the associated session, depending on how the application handles authentication and session security.

Therefore, session cookies should be protected using appropriate security controls such as:

```text
HTTPS
Secure
HttpOnly
SameSite
Session expiration
Session rotation
```

> **Important:** A cookie itself does not necessarily contain your username or password. In many applications, it contains an identifier that the server uses to find your session.

---

# Cookie vs Session

A useful distinction:

```text
COOKIE
   │
   └── Stored on the client/browser

SESSION
   │
   └── Usually maintained on the server
```

For example:

```text
Browser
   │
   │ Cookie:
   │ session-id=xyz123
   ▼
Server
   │
   │ Looks up:
   │ xyz123 → Junaid's session
   ▼
Authenticated Session
```

The exact implementation can vary. Some modern applications use other mechanisms, such as stateless tokens, rather than traditional server-side sessions.

---

# Conclusion

The core idea of cookies is simple:

```text
Server
   │
   │ Set-Cookie
   ▼
Browser stores cookie
   │
   │ Cookie
   ▼
Server receives cookie
   │
   │
   ▼
Server identifies the
associated session
```

In a typical login-based application:

```text
Login
  ↓
Server verifies credentials
  ↓
Server sends Set-Cookie
  ↓
Browser stores cookie
  ↓
Browser sends Cookie with later requests
  ↓
Server checks the session
  ↓
User remains authenticated
```

Understanding cookies is important for web security because authentication, sessions, CSRF, session hijacking, XSS, and many web vulnerabilities involve cookies or the way browsers handle them.
