TryHackMe: WebFortress-CTF Room - Official Write-Up

# Introduction

Welcome to the official write-up for the WebFortress-CTF Room, a comprehensive journey through common web application vulnerabilities. This room demonstrates how seemingly minor oversights in development can lead to significant security breaches, emphasizing the importance of secure coding practices and thorough security testing in modern web applications.

# Vulnerability Analysis and Exploitation
1. Reconnaissance – HTML Source Analysis

Developers sometimes leave comments or debug information in HTML source code. These can disclose technologies used, hidden parameters, or credentials.
Key Takeaway: Always remove sensitive comments from production code.

# 2. SQL Injection in Login Form

The login form concatenates user input directly into SQL queries without sanitization. This allows an attacker to manipulate the query logic and bypass authentication.

⚠️ Test Payload Example (Lab Use Only)

admin' OR '1'='1


This payload demonstrates a tautology-based SQL Injection attack used in training environments only.

Explanation:
The payload alters the SQL logic to always evaluate as true. Proper mitigations include parameterized queries (prepared statements), strong input validation, and least-privilege database accounts.

# 3. Insecure Direct Object Reference (IDOR)

By changing a predictable URL parameter such as user_id, an attacker can view another user’s information.
Example: /profile?user_id=101 → change to /profile?user_id=2.

Key Takeaway: Always perform server-side authorization checks and use indirect references rather than sequential IDs.

# 4. Path Traversal – Document Manager

Improper file path validation allows attackers to access files outside the intended directory.

⚠️ Test Payload Example (Lab Use Only)

....//....//....//....//etc/passwd
../../../..//etc/passwd


This payload demonstrates a typical directory traversal attack in a controlled lab.

Explanation:
Attackers can read sensitive system files. Mitigation includes using whitelist-based file access, validating input rigorously, and limiting file system privileges.

# 5. Cross-Site Scripting (XSS)

User input is reflected back without encoding, allowing malicious JavaScript execution.

⚠️ Test Payload Example (Lab Use Only)

<scr!pt>alert(1)</scr!pt>


This neutralized payload shows a reflected XSS attempt.

Explanation:
Mitigation includes context-aware output encoding, using Content Security Policy (CSP), and sanitizing input.

# 6. Privilege Escalation

Attackers can combine previous vulnerabilities to escalate privileges to administrative access. This highlights why individual flaws must be fixed quickly and why layered security controls matter.

Key Takeaway: Always implement robust role-based access control, test privilege boundaries regularly, and monitor for suspicious access patterns.

Additional Testing Areas

The Resource Search functionality and Document Manager warrant thorough testing for SQL Injection, XSS, and information disclosure. Hidden admin endpoints may exist and can be found using directory brute-forcing, reviewing page source, or analyzing JavaScript files.

Conclusion

The WebFortress-CTF room provides a realistic environment for understanding fundamental web application vulnerabilities and their exploitation techniques. By practicing in a controlled environment, security professionals gain insights into common flaws, proper testing methodologies, and effective defense strategies. This exercise reinforces that security is not a one-time feature but an ongoing process requiring continuous testing, defense-in-depth, and secure development practices.
