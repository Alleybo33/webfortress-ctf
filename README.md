# webfortress-ctf
TryHackMe: WebFortress-CTF Room - Official Write-Up

Introduction

Welcome to the official write-up for the Web Exploitation Room! This room is an excellent, hands-on learning experience designed to introduce you to some of the most common and critical web application vulnerabilities. By the end of this guide, you'll have a clear understanding of how to identify and exploit Cross-Site Scripting (XSS), SQL Injection (SQLi), Path Traversal, and Command Injection. Get ready to dive into the world of ethical hacking and see firsthand why input sanitization is a developer's best friend. 🔒💻

# Task 1: Cross-Site Scripting (XSS)
Vulnerability Analysis
The search bar on this page is a perfect example of a reflected XSS vulnerability. This happens when an application takes user input and immediately reflects it back to the page without proper sanitization. This allows an attacker to inject and execute malicious scripts in the victim's browser.

Exploitation
Navigate to the website's search bar.

Input the following payload:

 <script>alert(1)</script>
Click the "Search" button.

Observe the result: an alert box should pop up with the text "xss". This confirms the vulnerability is present.

Retrieve the flag from the alert box:

FLAG1{}

Alternative Payloads & Lessons Learned
While the basic payload is effective, understanding different types is key. For example, you could try:

"><script>alert(1)</script>: This payload breaks out of an HTML attribute and injects a script tag.

test" onmouseover="alert(1): This demonstrates how to inject an event handler into an existing HTML tag.

The key takeaway here is to always sanitize and validate all user input. Additionally, implementing a robust Content Security Policy (CSP) header is a crucial defense, as it tells the browser which sources are trusted to execute scripts.

# Task 2: SQL Injection (SQLi)
Vulnerability Analysis
The "User Lookup" form on this page is susceptible to SQL Injection, a serious vulnerability that occurs when an application constructs SQL queries using unsanitized user input. This allows an attacker to manipulate the query and gain unauthorized access to the database.

Exploitation
Navigate to the "User Lookup" form.

Enter the following payload into the input field:

1' UNION SELECT 1,2,3-- -
Click "Find User".

Observe the result: The database returns unintended data because the UNION operator was used to append a second, malicious query to the original one. The -- - at the end comments out the rest of the original query, preventing syntax errors.

Retrieve the flag from the returned data:

FLAG2{}

Lessons Learned
The most effective way to prevent SQL Injection is by using parameterized queries or prepared statements. These methods separate the SQL code from the user input, ensuring the input is treated as a literal value rather than an executable command.

# Task 3: Path Traversal
Vulnerability Analysis
The "Document Viewer" functionality has a Path Traversal vulnerability. This flaw allows an attacker to read arbitrary files from the server's file system by manipulating the file path with characters like ../ (dot-dot-slash) to navigate up the directory tree.

Exploitation
Navigate to the "Document Viewer" form.

Enter the following payload:

flag3.txt
Note: In a real-world scenario, you might need to use ../../flag3.txt or a full path like /var/www/html/flag3.txt to find the file.
../../../../etc/passwd
Click "View Document".

Observe the result: The server returns the contents of flag3.txt instead of a harmless file.

Retrieve the flag:

FLAG3{}

Lessons Learned
To prevent this, always validate and sanitize file paths. A whitelist-based access control system is ideal—it only allows access to a pre-defined list of files, effectively blocking any attempts to navigate outside of the designated directory.

# Task 4: Command Injection
Vulnerability Analysis
The "Network Diagnostics" form is vulnerable to Command Injection. This happens when an application executes a shell command constructed with unsanitized user input. An attacker can chain commands together, essentially tricking the server into running their own arbitrary commands.

Exploitation
Navigate to the "Network Diagnostics" form.

Enter the following payload:

google.com; ls
Click "Test Connection".

Observe the result: The application executes the ping google.com command as intended, but then the ; (semicolon) character acts as a command separator, allowing the ls command to also be executed. The server then returns the output of both commands.

Retrieve the flag:

FLAG4{}

Alternative Payloads & Lessons Learned
Other common command separators you could try include | (pipe) or &&. For example:

google.com; ls

google.com | whoami: Executes whoami, revealing the user the server is running as.

google.com; cat /etc/passwd: Attempts to read the sensitive passwd file.

The best defense against this is to never execute user input directly in shell commands. Instead, use secure, built-in APIs for specific tasks, and apply strict input validation.

Conclusion
This room provided a great foundation for understanding fundamental web vulnerabilities. You've now seen how simple failures in input validation and sanitization can lead to critical security flaws like XSS, SQLi, Path Traversal, and Command Injection. Remember these lessons as you continue your cybersecurity journey and strive to build or defend applications that are secure by design. What other vulnerabilities are you curious to explore?
