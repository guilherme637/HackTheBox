# Hack The Box — TimeKORP (Write-Up)

## Overview
The **TimeKORP** challenge, available on the Hack The Box platform, presents a web application related to the digital environment of the fictional company KROP.  
The objective of the challenge is to analyze how user-controlled input is processed by the backend and to identify potential security weaknesses arising from improper input handling.

This document focuses on the learning process, vulnerability classification, and security impact, without disclosing sensitive technical details.

---

## Objective
- Analyze the behavior of a web application
- Identify insecure handling of user input
- Understand and classify an injection vulnerability
- Assess the potential impact on the underlying system

---

## Initial Analysis
During the initial interaction with the application, a specific HTTP parameter was identified as responsible for controlling the response format or processing logic.

This parameter appeared to be directly handled by the backend without visible validation or sanitization mechanisms, making it a potential attack vector.

---

## Vulnerability Identification
By manipulating the identified parameter, it was possible to alter the expected behavior of the application.

The backend began interpreting part of the user input as instructions executed by the operating system. This behavior confirmed the presence of an **OS Command Injection** vulnerability, allowing arbitrary command execution under the context of the running service.

---

## Technical Concepts Involved
- Improper validation of user-controlled input
- Interaction between web applications and the operating system
- Unsafe execution of system-level commands
- Importance of input sanitization and secure coding practices

---

## Tools and Skills Used
- Web browser for request analysis
- Understanding of HTTP request/response flow
- Basic knowledge of Linux systems
- Manual testing and logical analysis

---

## Vulnerability Classification
- **Vulnerability Type:** OS Command Injection
- **OWASP Top 10:** A03 – Injection
- **CWE:** CWE-78 – Improper Neutralization of Special Elements used in an OS Command

---

## Impact Assessment
If exploited in a real-world scenario, this vulnerability could allow:
- Execution of arbitrary system commands
- Access to sensitive files
- Further system compromise depending on privilege level
- Potential full takeover of the affected server

---

## Mitigation Recommendations
- Avoid executing system commands with user-supplied input
- Use secure APIs that do not invoke a shell
- Apply strict input validation using whitelists
- Properly escape or sanitize special characters
- Enforce the principle of least privilege for application services

---

## Conclusion
The **TimeKORP** challenge highlights how insufficient input validation can lead to critical security vulnerabilities.

From a learning perspective, this laboratory reinforced the importance of careful input handling and demonstrated how small implementation oversights can result in significant security risks.
The exercise contributed to the development of analytical thinking and foundational skills essential for professionals at an early stage in cybersecurity.

---

## Key Takeaway
Seemingly simple application parameters can become critical attack vectors when they are trusted without proper validation.

