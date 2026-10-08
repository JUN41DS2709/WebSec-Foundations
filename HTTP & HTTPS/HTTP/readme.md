## HTTP — Hypertext Transfer Protocol

HTTP (Hypertext Transfer Protocol) is an application-layer protocol used for communication between clients and web servers.

It is one of the fundamental protocols used by the World Wide Web.

---

### What is HTTP?

> HTTP stands for **Hypertext Transfer Protocol**, which helps in communication between the client and the server.

HTTP works primarily over **TCP** in traditional HTTP/1.x and HTTP/2 connections.

The default port for HTTP is:

```text
TCP/80
```

For HTTPS, the default port is:

```text
TCP/443
```

### Basic HTTP Communication

```text
Client
  │
  │ HTTP Request
  ▼
Web Server
  │
  │ HTTP Response
  ▼
Client
```

For example:

```text
Browser
   │
   │ GET /index.html HTTP/1.1
   ▼
Web Server
   │
   │ HTTP/1.1 200 OK
   │ <HTML page>
   ▼
Browser
```

> **Note:** HTTP itself is an application-layer protocol. TCP operates at the transport layer and provides reliable transport for traditional HTTP connections.

---

## HTTP Messages

HTTP messages are data exchanged between a client and a web server.

These messages are very important for understanding how web applications work because they show how users' requests and the server's responses are communicated.

There are two main types of HTTP messages:

1. **HTTP Requests**
2. **HTTP Responses**

---

## HTTP Requests

An **HTTP request** is what a user sends to a web server to interact with a web application and make something happen.

HTTP requests are sent by the client to the server.

For example:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
```

The request tells the server:

- What resource the client wants
- What HTTP method is being used
- Additional information about the request
- Sometimes data that the client wants to send

---

## HTTP Request Structure

A typical HTTP request can contain:

```text
Request Line
Headers
Blank Line
Request Body (optional)
```

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

username=admin&password=test
```

---

## Request Headers

Request Headers allow extra information to be conveyed to the web server about the request.

Some common headers are as follows:

### Common Request Headers

| Request Header | Example | Description |
|---|---|---|
| **Host** | `Host: tryhackme.com` | Specifies the name of the web server the request is for. |
| **User-Agent** | `User-Agent: Mozilla/5.0` | Shares information about the client software making the request, commonly a web browser. |
| **Referer** | `Referer: https://www.google.com/` | Indicates the URL from which the request originated. |
| **Cookie** | `Cookie: user_type=student; room=introtowebapplication` | Contains cookies previously stored by the browser and sent to the server. |
| **Content-Type** | `Content-Type: application/json` | Describes the media type/format of the data contained in the request body. |

> **Note:** `Referer` is intentionally spelled this way in the HTTP specification, despite the usual spelling being "Referrer."

---

## HTTP Methods

HTTP methods (also known as HTTP verbs) indicate the specific action a client wants to perform on a server resource.

They form an important part of web communication and RESTful APIs.

Common HTTP methods include:

```text
GET
POST
PUT
PATCH
DELETE
```

---

## The 5 Most Common Methods

These methods are commonly associated with CRUD operations:

| Method | CRUD Action | Description |
|---|---|---|
| **GET** | Read | Retrieves data from a server. Parameters are commonly passed in the URL query string. GET is intended to be safe and should not modify server state. |
| **POST** | Create | Submits data to a server, commonly to create a resource or trigger an operation. Repeating a POST request may create multiple resources. |
| **PUT** | Update / Replace | Replaces the current representation of a resource with the supplied representation. |
| **PATCH** | Update | Applies partial modifications to an existing resource. |
| **DELETE** | Delete | Requests that a specified resource be deleted. |

### Example

Imagine an API managing users:

```text
GET     /users/10
POST    /users
PUT     /users/10
PATCH   /users/10
DELETE  /users/10
```

Possible interpretation:

```text
GET /users/10
        ↓
Retrieve user 10

POST /users
        ↓
Create a new user

PUT /users/10
        ↓
Replace user 10

PATCH /users/10
        ↓
Modify part of user 10

DELETE /users/10
        ↓
Delete user 10
```

> **Important:** HTTP methods describe the intended semantics of a request. The actual behavior depends on how the web application implements the endpoint.

---

## HTTP Responses

HTTP responses are sent by the server in response to the user's request.

When you interact with a web application, the server sends back an **HTTP response** to let you know whether your request was successful or something went wrong.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1256

<html>
    ...
</html>
```

---

## HTTP Response Structure

A typical HTTP response contains:

```text
Status Line
Headers
Blank Line
Response Body (optional)
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<h1>Hello World</h1>
```

---

## Status Line

The first line in an HTTP response is called the **Status Line**.

It gives you three key pieces of information:

1. **HTTP Version** — tells you which version of HTTP is being used.
2. **Status Code** — a three-digit number showing the outcome of the request.
3. **Reason Phrase** — a short human-readable description of the status.

Example:

```http
HTTP/1.1 200 OK
```

Breaking it down:

```text
HTTP/1.1 → HTTP Version
200      → Status Code
OK       → Reason Phrase
```

> **Note:** In HTTP/2 and HTTP/3, the wire representation is different and does not use the traditional textual status line in the same way. The concept of a status code still exists.

---

## HTTP Status Codes

Status codes are **three-digit numbers sent by a server in response to a client's request**.

Examples:

```text
1xx
2xx
3xx
4xx
5xx
```

They are divided into five major categories:

```text
1xx → Informational
2xx → Successful
3xx → Redirection
4xx → Client Errors
5xx → Server Errors
```

---

## 1xx — Informational Responses

These responses provide information about the request or communication process.

They generally indicate that the server has received the request and the client may need to continue the exchange.

Example:

```text
100 → Continue
```

### 100 — Continue

The server received the initial part of the request and indicates that the client can continue sending the request.

---

## 2xx — Successful Responses

These status codes indicate that the request was successfully received, understood, and processed.

Common examples:

```text
200 → OK

201 → Created

202 → Accepted

203 → Non-Authoritative Information
```

### 200 — OK

The request was successful.

Example:

```http
GET /index.html HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK
```

---

### 201 — Created

The request was successful and resulted in a new resource being created.

Commonly seen after:

```http
POST /users
```

Response:

```http
HTTP/1.1 201 Created
```

---

### 202 — Accepted

The request has been accepted for processing, but the processing may not have completed yet.

---

### 203 — Non-Authoritative Information

The request was successful, but the returned metadata may not exactly match what would have been received directly from the origin server.

---

## 3xx — Redirection Responses

These status codes indicate that the client needs to take additional action, often involving a different URL or cached representation.

Common examples:

```text
301 → Moved Permanently

302 → Found

303 → See Other

304 → Not Modified
```

### 301 — Moved Permanently

The requested resource has been permanently assigned a new URL.

Example:

```text
http://example.com
        ↓
https://example.com
```

The server may respond with:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com
```

---

### 302 — Found

The requested resource is temporarily available at another location.

---

### 303 — See Other

The response indicates that the client should retrieve the resource using another URI, typically with a `GET` request.

---

### 304 — Not Modified

The requested resource has not changed since the version stored in the client's cache.

This allows the browser to reuse its cached copy instead of downloading the resource again.

---

## 4xx — Client Error Responses

These status codes indicate that there was a problem with the request sent by the client.

Common examples:

```text
400 → Bad Request

401 → Unauthorized

403 → Forbidden

404 → Not Found

405 → Method Not Allowed

429 → Too Many Requests
```

### 400 — Bad Request

The server could not understand or process the request because the request was invalid.

Example:

```http
GET /users?id=??? HTTP/1.1
```

The server may respond:

```http
HTTP/1.1 400 Bad Request
```

---

### 401 — Unauthorized

The request requires authentication.

For example:

```http
GET /admin HTTP/1.1
```

The server may respond:

```http
HTTP/1.1 401 Unauthorized
```

> Despite the name, `401` generally means **authentication is required or has failed**, rather than simply "you don't have permission."

---

### 403 — Forbidden

The server understood the request but refuses to authorize it.

Example:

```http
GET /admin-panel HTTP/1.1
```

Response:

```http
HTTP/1.1 403 Forbidden
```

---

### 404 — Not Found

The server could not find a current representation of the requested resource.

Example:

```http
GET /does-not-exist HTTP/1.1
```

Response:

```http
HTTP/1.1 404 Not Found
```

---

### 405 — Method Not Allowed

The requested HTTP method is known by the server but is not allowed for that particular resource.

Example:

```http
DELETE /index.html HTTP/1.1
```

The server may respond:

```http
HTTP/1.1 405 Method Not Allowed
Allow: GET, HEAD
```

---

### 429 — Too Many Requests

The client has sent too many requests within a given period.

This is commonly associated with:

- Rate limiting
- API limits
- Brute-force protection
- Automated request restrictions

Example:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
```

---

## 5xx — Server Error Responses

These status codes indicate that the server encountered a problem while attempting to fulfil a valid request.

Common examples:

```text
500 → Internal Server Error

502 → Bad Gateway

503 → Service Unavailable

504 → Gateway Timeout

505 → HTTP Version Not Supported
```

### 500 — Internal Server Error

Something went wrong on the server while processing the request.

Example:

```http
HTTP/1.1 500 Internal Server Error
```

---

### 502 — Bad Gateway

A server acting as a gateway or proxy received an invalid response from an upstream server.

Example:

```text
Client
  ↓
Reverse Proxy
  ↓
Application Server
```

If the application server responds incorrectly, the proxy may return:

```http
HTTP/1.1 502 Bad Gateway
```

---

### 503 — Service Unavailable

The server is currently unable to handle the request.

Possible reasons include:

- Server overload
- Maintenance
- Temporary unavailability

---

### 504 — Gateway Timeout

A server acting as a gateway or proxy did not receive a timely response from an upstream server.

---

### 505 — HTTP Version Not Supported

The server does not support the HTTP version used in the request.

---

## HTTP Request and Response — Complete Example

Let's put everything together.

## Client Request

```http
GET /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123
```

The client is essentially saying:

```text
"Give me /login from example.com."
```

---

## Server Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 42

<h1>Welcome to the login page</h1>
```

The server is essentially saying:

```text
"Your request was successful.
Here is the requested resource."
```

---

## HTTP Communication Flow

A simplified web request looks like this:

```text
             HTTP Request
Client ─────────────────────────► Server
       GET /index.html

             HTTP Response
Client ◄───────────────────────── Server
       HTTP/1.1 200 OK
       <HTML>
```

---

## HTTP and Cybersecurity

Understanding HTTP is extremely important in cybersecurity because modern web applications rely heavily on HTTP.

As a penetration tester or security researcher, you will frequently analyze:

```text
HTTP Requests
HTTP Responses
HTTP Methods
Headers
Cookies
Authentication
Sessions
Status Codes
Parameters
Request Bodies
Response Bodies
```

For example, a penetration tester may inspect:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=test
```

The tester can then study how the application processes:

```text
username
password
authentication
cookies
sessions
status codes
```

This forms the foundation for understanding web vulnerabilities and web application security testing.

---

## Quick Reference

## HTTP

```text
HTTP
│
├── Application Layer Protocol
├── Commonly uses TCP
├── Default HTTP port → TCP/80
└── HTTPS → TCP/443
```

## HTTP Messages

```text
HTTP Messages
│
├── Request
│   ├── Request Line
│   ├── Headers
│   ├── Blank Line
│   └── Body (optional)
│
└── Response
    ├── Status Line
    ├── Headers
    ├── Blank Line
    └── Body (optional)
```

## HTTP Methods

```text
GET
POST
PUT
PATCH
DELETE
```

## Status Codes

```text
1xx → Informational

2xx → Successful

3xx → Redirection

4xx → Client Errors

5xx → Server Errors
```

## Important Status Codes

```text
200 → OK
201 → Created

301 → Moved Permanently
302 → Found
304 → Not Modified

400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
405 → Method Not Allowed
429 → Too Many Requests

500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
504 → Gateway Timeout
505 → HTTP Version Not Supported
```

---

## Summary

HTTP is the protocol that enables communication between web clients and servers.

A client sends an **HTTP request**, and the server returns an **HTTP response**.

```text
Client
   │
   │ HTTP Request
   ▼
Server
   │
   │ HTTP Response
   ▼
Client
```

Requests contain information such as:

```text
Method
URL
Headers
Body
```

Responses contain:

```text
Status Code
Headers
Body
```

HTTP methods define the intended operation:

```text
GET
POST
PUT
PATCH
DELETE
```

HTTP status codes communicate the result:

```text
1xx → Information
2xx → Success
3xx → Redirection
4xx → Client Error
5xx → Server Error
```

---

### Conclusion

HTTP is one of the most important foundations of web application security.

Before learning vulnerabilities such as:

```text
SQL Injection
XSS
CSRF
SSRF
IDOR
Authentication Bypass
Command Injection
File Upload Vulnerabilities
```

you need to understand how HTTP requests and responses work.

A security professional should eventually be comfortable looking at raw HTTP traffic and understanding exactly what the client is requesting, what the server is returning, and how the application behaves in response to different inputs.

This README establishes the foundation for deeper **web application penetration testing and offensive security**.
