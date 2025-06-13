# Web Attacks: SQL Injection, Cross-Site Scripting (XSS), and CSRF

This section explains three common web application vulnerabilities: **SQL Injection (SQLi)**, **Cross-Site Scripting (XSS)**, and **Cross-Site Request Forgery (CSRF)**. These attacks exploit insecure input handling and user sessions.

---

## 1. SQL Injection (SQLi)

SQL injection occurs when an attacker inserts malicious SQL code into a web form or URL to gain unauthorized access to the database.

### Example:

```sql
SELECT * FROM users WHERE username = '$user' AND password = '$pass';
```

If `$user` = `' OR '1'='1` and `$pass` = anything, the query becomes:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = 'anything';
```

This condition is always true and can allow access without a password.

### Goals of SQLi:

- Bypass login/authentication
- View, modify, or delete database records
- Dump entire tables or read system files
- In advanced cases: gain remote shell access

### Prevention:

- Use **parameterized queries** or **prepared statements**
- Validate and sanitize all user inputs
- Limit database privileges for web apps

---

## 2. Cross-Site Scripting (XSS)

XSS allows attackers to inject malicious scripts (usually JavaScript) into trusted websites. These scripts are then executed in the browsers of other users.

### Example:

A forum allows HTML tags and does not filter input. An attacker posts:

```html
<script>alert('Hacked');</script>
```

Anyone viewing the post sees a popup — or worse, has their cookies/session stolen.

### Types of XSS:

- **Stored**: Script is permanently saved on server (e.g. in a comment field)
- **Reflected**: Script is in the URL and reflected back in server response
- **DOM-based**: Script is executed client-side due to dynamic JavaScript updates

### Prevention:

- Sanitize user inputs
- Escape HTML/JS output
- Use Content Security Policy (CSP)
- Avoid directly injecting user data into the DOM

---

## 3. Cross-Site Request Forgery (CSRF)

CSRF tricks an authenticated user into performing actions without their consent.

### Example:

If a user is logged in to `bank.com`, and visits:

```html
<img src="http://bank.com/transfer?amount=1000&to=attacker" />
```

The browser automatically includes session cookies, so the request appears legitimate — money is transferred.

### Prevention:

- Use anti-CSRF tokens (random tokens tied to user sessions)
- Require re-authentication for sensitive actions
- Use SameSite cookie attributes

---

## Summary Comparison

| Attack Type | Target         | Method                            | Goal                              |
|-------------|----------------|-----------------------------------|-----------------------------------|
| SQLi        | Database       | Malicious input in SQL fields     | Unauthorized data access          |
| XSS         | Browser/User   | Injected JavaScript in web pages  | Cookie theft, phishing, defacement|
| CSRF        | Web Application| Tricked requests using sessions   | Force user to perform actions     |

---

Understanding these vulnerabilities helps developers build more secure applications and helps analysts recognize exploit techniques during testing.

