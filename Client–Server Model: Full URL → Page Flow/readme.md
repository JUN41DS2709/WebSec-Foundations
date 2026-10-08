# Client–Server Model: Full URL → Page Flow

Suppose you type:

```text
https://example.com/login
```

into your browser and press **Enter**.

The complete journey looks roughly like this:

```text
                    INTERNET
                       │
                       ▼
┌─────────────┐   1. DNS query    ┌─────────────┐
│   Browser   │ ────────────────► │ DNS Server  │
│  (CLIENT)   │ ◄──────────────── │             │
└──────┬──────┘   IP address      └─────────────┘
       │
       │ 2. TCP connection
       ▼
┌─────────────┐
│ Web Server  │
│  (SERVER)   │
└──────┬──────┘
       │
       │ 3. HTTP request
       ▼
┌─────────────┐
│ Application │
│   Backend   │
└──────┬──────┘
       │
       │ 4. Database query
       ▼
┌─────────────┐
│  Database   │
└──────┬──────┘
       │
       │ 5. Data
       ▼
┌─────────────┐
│ Application │
│   Backend   │
└──────┬──────┘
       │
       │ 6. HTTP response
       ▼
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │
       │ 7. Render HTML/CSS/JS
       ▼
       🌐 PAGE
```

---

# 1. You enter the URL

You type:

```text
https://example.com/login
```

A URL has several important parts:

```text
https://example.com/login
  │        │          │
  │        │          └── Path
  │        └───────────── Domain
  └────────────────────── Protocol
```

More precisely:

```text
https://example.com:443/login
        │          │
        │          └── Port
        └──────────── Host
```

Because HTTPS normally uses port **443**, so we usually don't need to type it.

---

# 2. Browser needs the server's IP address

computer understands network addresses such as:

```text
142.250.x.x
```

but we typed:

```text
example.com
```

So the browser needs **DNS**.

DNS means:

> Domain Name System

Think of it as:

```text
example.com
     ↓
   DNS
     ↓
IP address
```

For example:

```text
example.com
     ↓
93.184.216.34
```

The browser can now figure out where to send the traffic.

---

# 3. Browser establishes a connection

Because we're using HTTPS:

```text
https://
```

the browser needs a secure connection to the server.

At the network level, we'll typically have:

```text
Client
   │
   │ TCP connection
   ▼
Server
```

For HTTPS, TLS is then negotiated:

```text
Browser ───── TCP ─────► Server
Browser ◄─── TCP ────── Server

Browser ───── TLS ─────► Server
Browser ◄──── TLS ───── Server
```

The result is an encrypted channel.

Conceptually:

```text
Browser
   │
   │ 🔒 encrypted HTTPS
   ▼
Server
```

This is why someone sitting on the network generally can't simply read the contents of your HTTPS request.

---

# 4. Browser sends an HTTP request

Now the interesting part.

The browser sends something like:

```http
GET /login HTTP/1.1
Host: example.com
User-Agent: Firefox
Accept: text/html
```

This is an **HTTP request**.

The important piece is:

```http
GET /login
```

It means:

> "Server, give me the resource at `/login`."

---

# 5. Server receives the request

The request reaches the server.

For example:

```text
Client
  │
  │ GET /login
  ▼
Web Server
```

The web server might be something like:

```text
Nginx
Apache
```

or an application server/framework such as:

```text
Flask
Django
Node.js
Spring
ASP.NET
```

In your **CyberLab**, you are currently using Flask.

Your Flask application might have:

```python
@app.route("/login")
def login():
    ...
```

So Flask sees:

```text
GET /login
```

and finds the corresponding route.

---

# 6. Backend may talk to a database

Suppose `/login` needs information from a database.

The application might communicate with:

```text
Flask
  │
  │ SQL query
  ▼
MySQL
```

For example, conceptually:

```sql
SELECT username FROM users WHERE id = 10;
```

The database processes the query and returns data.

```text
Database
    │
    │ result
    ▼
Flask
```

Important distinction:

**The browser normally does not directly talk to your database.**

Usually:

```text
Browser → Backend → Database
```

not:

```text
Browser → Database
```

This separation is extremely important for web security.

---

# 7. Backend generates a response

The backend takes the information it needs and creates an HTTP response.

For example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

followed by HTML:

```html
<html>
    <body>
        <h1>Login</h1>
    </body>
</html>
```

The server sends this back:

```text
Server
   │
   │ HTTP response
   ▼
Browser
```

---

# 8. Browser receives the response

The browser receives:

```html
<html>
    <body>
        <h1>Login</h1>
    </body>
</html>
```

But HTML alone isn't necessarily the whole page.

The browser may discover additional resources:

```html
<link rel="stylesheet" href="/style.css">
<script src="/script.js"></script>
<img src="/logo.png">
```

Now the browser makes additional requests.

```text
GET /login
       ↓
    HTML

GET /style.css
       ↓
    CSS

GET /script.js
       ↓
 JavaScript

GET /logo.png
       ↓
    Image
```

So one page load can actually involve **many HTTP requests**.

---

# 9. Browser renders the page

Finally, the browser processes everything:

```text
HTML
 ↓
DOM

CSS
 ↓
Styles
 ↓
JavaScript
 ↓
Behavior
```

and produces the visual webpage:

```text
┌──────────────────────────────┐
│          CyberLab            │
│                              │
│       Username: ______       │
│       Password: ______       │
│                              │
│          [ Login ]           │
└──────────────────────────────┘
```

That's the page you see.

---

# The Entire Flow

Put everything together:

```text
YOU
 │
 │ Type https://example.com/login
 ▼
BROWSER
 │
 │ DNS lookup
 ▼
DNS
 │
 │ IP address
 ▼
BROWSER
 │
 │ TCP connection
 ▼
SERVER
 │
 │ TLS handshake
 ▼
🔒 HTTPS CONNECTION
 │
 │ GET /login
 ▼
WEB SERVER
 │
 ▼
BACKEND APPLICATION
 │
 │ Need user/data?
 ▼
DATABASE
 │
 │ Data
 ▼
BACKEND
 │
 │ HTTP response
 ▼
BROWSER
 │
 │ HTML
 │ CSS
 │ JavaScript
 │ Images
 ▼
RENDERING ENGINE
 │
 ▼
🌐 WEB PAGE
```

**Browser → HTTP request → Flask route → application logic → database → HTTP response → browser.**

---

# Example: What Actually Happens When You Visit `/login`

A simplified version of the complete process:

### Step 1 — URL

```text
https://example.com/login
```

### Step 2 — DNS

```text
example.com
      ↓
93.184.216.34
```

### Step 3 — Connection

```text
Browser ── TCP ──► Server
Browser ◄─ TCP ─── Server

Browser ── TLS ──► Server
Browser ◄─ TLS ─── Server
```

### Step 4 — Request

```http
GET /login HTTP/1.1
Host: example.com
```

### Step 5 — Backend

```text
Web Server
    ↓
Flask
    ↓
/login route
    ↓
Application logic
```

### Step 6 — Database, if required

```text
Flask
  ↓
SQL query
  ↓
MySQL
  ↓
Result
  ↓
Flask
```

### Step 7 — Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

```html
<h1>Login</h1>
```

### Step 8 — Browser

```text
HTML
CSS
JavaScript
Images
   ↓
Browser rendering engine
   ↓
🌐 Web page
```

---

# Important Security Perspective

Understanding this flow is extremely important for **web application security**.

Each layer introduces different things that a security professional needs to understand:

```text
DNS
 │
 ├── DNS enumeration
 ├── DNS misconfiguration
 └── DNS attacks

TCP / TLS
 │
 ├── Ports
 ├── Connections
 ├── Encryption
 └── Certificate validation

HTTP
 │
 ├── Requests
 ├── Responses
 ├── Headers
 ├── Cookies
 └── Authentication

Backend
 │
 ├── Routes
 ├── Authentication
 ├── Authorization
 ├── Input validation
 └── Business logic

Database
 │
 ├── SQL
 ├── Queries
 ├── Access control
 └── Data protection

Browser
 │
 ├── JavaScript
 ├── DOM
 ├── Cookies
 ├── Storage
 └── Same-Origin Policy
```

This is why learning the **client–server model** is one of the foundations of web penetration testing.

---

# Key Takeaways

```text
1. User enters a URL
          ↓
2. Browser resolves the domain using DNS
          ↓
3. Browser establishes a network connection
          ↓
4. TLS secures the HTTPS connection
          ↓
5. Browser sends an HTTP request
          ↓
6. Web server receives the request
          ↓
7. Backend processes the request
          ↓
8. Backend may communicate with a database
          ↓
9. Backend generates an HTTP response
          ↓
10. Browser receives the response
          ↓
11. Browser requests additional resources
          ↓
12. Browser renders the final page
```

### The most important mental model

```text
                    WEB APPLICATION

Browser
   │
   │ HTTP/HTTPS
   ▼
Web Server
   │
   ▼
Backend Application
   │
   ▼
Database
```

The browser is the **client**.

The server-side application is the **server**.

The browser communicates with the application primarily through **HTTP/HTTPS**.

And the backend communicates with other components, such as databases, to process requests and generate responses.

---

# Conclusion

When you type:

```text
https://example.com/login
```

you aren't simply "opening a webpage."

A series of network and application-level operations happens:

```text
URL
 ↓
DNS
 ↓
IP address
 ↓
TCP
 ↓
TLS
 ↓
HTTPS
 ↓
HTTP Request
 ↓
Web Server
 ↓
Backend
 ↓
Database
 ↓
Backend
 ↓
HTTP Response
 ↓
Browser
 ↓
HTML/CSS/JavaScript
 ↓
🌐 Web Page
```

Understanding this entire chain gives you the foundation for understanding how modern web applications actually work — and later, how they can be tested and attacked from a security perspective.
