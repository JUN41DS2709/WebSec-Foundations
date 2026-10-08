## INTRODUCTION TO WEB APPLICATION

### What is a Web Application?

A **website** can primarily provide information.

A **web application** is an interactive software program that is accessed through a web browser over a network such as the internet.

A **web application** allows the user to interact with data and functionality.

For example, Instagram:

```text
Login
 ↓
View profile
 ↓
Upload image
 ↓
Like post
 ↓
Send message
 ↓
Change settings
```

All of those actions require the browser to communicate with **backend systems**.

---

## FRONTEND

The **frontend** is the part of a website or application that people see and interact with directly.

**HTML, CSS, and JavaScript** are three foundational technologies of the World Wide Web. They work together in a browser to create everything from simple web pages to complex interactive web applications.

### 1. HTML

- HTML stands for **HyperText Markup Language**.
- It is a markup language used to create the **structure** of a web page or application.
- It defines elements such as headings, paragraphs, images, links, tables, forms, and more.

### 2. CSS

- CSS stands for **Cascading Style Sheets**.
- CSS is used to control the **style and presentation** of HTML elements.
- It controls things such as fonts, spacing, colors, alignment, layouts, and responsiveness across different devices.

### 3. JavaScript

- JavaScript is used to provide **behavior and interactivity** to web pages and applications.
- It allows features such as buttons being clicked, pop-ups opening, forms being validated, and data being loaded dynamically without necessarily refreshing the entire page.

---

# URL

**URL** stands for **Uniform Resource Locator**.

A **URL**, commonly known as a **web address**, is a specific character string that references a resource on the internet.

It specifies the location of a resource and the protocol or scheme used to access it.

A URL can be used to access different types of online resources, such as:

- Web pages
- Images
- Videos
- Documents
- Other web resources

For example:

```text
https://example.com:443/path/page?id=10#section
```

A URL can contain several different components.

---

## SCHEME

The **scheme** specifies the protocol used to access the resource.

Common schemes include:

- **HTTP** — HyperText Transfer Protocol
- **HTTPS** — HyperText Transfer Protocol Secure

HTTPS uses encryption to protect data exchanged between the browser and the server.

Example:

```text
https://example.com
^^^^^
Scheme
```

---

## USERINFO

Some URLs can contain **userinfo**, such as a username and historically a password, for resources that require authentication.

Example:

```text
https://username@example.com
        ^^^^^^^^
        Userinfo
```

Including credentials in a URL is generally unsafe because URLs can be exposed through browser history, logs, and other systems. Therefore, this is rarely used in modern web applications.

---

## HOST / DOMAIN

The **host** identifies the server or destination that the browser is connecting to.

A domain name is a human-readable name used to identify a service or website.

Example:

```text
https://example.com
         ^^^^^^^^^^^
         Host / Domain
```

From a security standpoint, pay attention to domain names that look almost identical to legitimate domains but contain small differences. This technique is known as **typosquatting**.

Typosquatted domains can be used in **phishing attacks** to trick users into providing sensitive information.

---

## PORT

The **port number** helps identify the specific network service that should receive the connection on a server.

It can be thought of as a specific "doorway" used for network communication.

Port numbers range from **0 to 65,535**.

Common web-related ports include:

- **80** — HTTP
- **443** — HTTPS

Example:

```text
https://example.com:443
                  ^^^
                 Port
```

---

## PATH

The **path** identifies a specific resource or route within a website or web application.

Example:

```text
https://example.com/products
                   ^^^^^^^^
                     Path
```

The path does not necessarily correspond to a physical file on the server. In modern web applications, it can represent a **route handled by the application**.

---

## QUERY STRING

The **query string** is the part of a URL that starts with a question mark (`?`).

It is commonly used to send parameters to a web application, such as search terms, filters, or other input.

Example:

```text
https://example.com/search?q=books
                         ^^^^^^^^^
                         Query String
```

Users can modify query parameters, so web applications must properly validate and handle this input.

Improper handling of user-controlled input can contribute to vulnerabilities such as **injection attacks**.

---

## FRAGMENT

The **fragment** starts with a hash symbol (`#`).

It is commonly used to identify a specific section or location within a web page.

Example:

```text
https://example.com/page#contact
                           ^^^^^^^
                           Fragment
```

For example, a fragment can allow the browser to jump directly to a particular heading or section of a page.

```text
https://example.com/document#introduction
```

Here, `#introduction` identifies the **Introduction** section of the document.
