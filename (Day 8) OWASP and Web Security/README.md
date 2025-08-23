
---

# 📌 Web Security Concepts

## 🔹 1. OWASP

**OWASP (Open Web Application Security Project)** is a nonprofit organization focused on improving software security.
It provides free, community-driven resources like documentation, tools, and training for developers and security professionals.

The **most popular resource** from OWASP is the **OWASP Top 10**, a standard awareness document listing the **top 10 critical web application security risks**.

---

## 🔹 2. OWASP Top 10 (2021 edition)

Here are the categories with explanations:

1. **Broken Access Control**

   * When users can access data or functions they shouldn’t (e.g., modifying another user’s account).
   * Example: URL tampering to view another user’s order details.

2. **Cryptographic Failures**

   * Weak or improper use of encryption, leading to data leaks.
   * Example: Storing passwords in plain text or using outdated ciphers.

3. **Injection**

   * Unsanitized input is passed into interpreters like SQL, NoSQL, LDAP, OS commands.
   * Example: SQL Injection (`' OR 1=1 --`).

4. **Insecure Design**

   * Flaws in system architecture and design.
   * Example: No rate-limiting on login attempts, making brute force possible.

5. **Security Misconfiguration**

   * Using default credentials, unnecessary services, verbose error messages.
   * Example: Leaving the `/admin` panel open with default login.

6. **Vulnerable and Outdated Components**

   * Using old libraries, frameworks, or plugins with known vulnerabilities.
   * Example: Using an outdated version of Apache Struts that has RCE bugs.

7. **Identification and Authentication Failures**

   * Weak authentication, poor session handling, or credential flaws.
   * Example: Session IDs exposed in URLs, or no MFA (Multi-Factor Authentication).

8. **Software and Data Integrity Failures**

   * Using code or updates without verifying integrity/signature.
   * Example: Dependency confusion attacks on package managers.

9. **Security Logging and Monitoring Failures**

   * No proper logging or monitoring, making attacks hard to detect.
   * Example: No alerts when multiple failed logins occur.

10. **Server-Side Request Forgery (SSRF)**

    * Application fetches remote resources without validating the URL.
    * Example: Attacker makes the server request internal cloud metadata endpoints.

---

## 🔹 3. Types of Injection Attacks

Injection attacks occur when **untrusted input** is inserted into a program and executed.

### Common Types:

1. **SQL Injection (SQLi)**

   * Manipulating SQL queries via user input.
   * Example:

     ```sql
     SELECT * FROM users WHERE username = 'admin' OR '1'='1';
     ```

2. **NoSQL Injection**

   * Targets NoSQL databases (like MongoDB).
   * Example: `{ "username": { "$ne": null }, "password": { "$ne": null } }`

3. **Command Injection**

   * Executing OS commands via input.
   * Example: `; rm -rf /` appended in a vulnerable shell command.

4. **LDAP Injection**

   * Manipulating LDAP queries.
   * Example: `(&(uid=*)(userPassword=*))` returns all users.

5. **XML Injection / XXE (XML External Entity)**

   * Injecting malicious XML data to read files or cause DoS.
   * Example:

     ```xml
     <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
     ```

---

## 🔹 4. Authentication vs Authorization

These two are often confused but are **different security concepts**:

* **Authentication (AuthN)**

  * Verifies **who you are**.
  * Example: Logging in with username + password, fingerprint, or OTP.
  * Think of **a security guard checking your ID**.

* **Authorization (AuthZ)**

  * Decides **what you can access** after authentication.
  * Example: A normal user cannot access the admin panel.
  * Think of **permissions on what areas you can enter after ID check**.

---

## 🔹 5. `robots.txt`

* A simple **text file** placed in the **root directory of a website** (`example.com/robots.txt`).
* Used to tell **web crawlers/spiders** (like Googlebot) which pages or files should **not** be crawled or indexed.

Example:

```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
```

* This doesn’t enforce security (attackers can still view it) — it’s just a guideline for search engine bots.
* Hackers often check `robots.txt` to find hidden endpoints.

---

## 🔹 6. Web Spiders (Web Crawlers)

* **Web spiders (crawlers, bots)** are automated programs that **browse the internet systematically** to index content.
* Example: Googlebot (used for Google Search indexing).

### How they work:

1. Start from a URL (seed).
2. Crawl the page and extract all hyperlinks.
3. Add new links to the crawl queue.
4. Follow the site’s `robots.txt` and meta tags (if not malicious).
5. Store indexed content in a database for search engines.

### Uses:

* **Search engines**: indexing web pages.
* **Data scraping**: collecting product prices, research data.
* **Security scanners**: checking vulnerabilities across websites.

---


Do you want me to also make a **diagram-style cheat sheet** for this (like a one-page revision note in Markdown with arrows and bullet flow)?
