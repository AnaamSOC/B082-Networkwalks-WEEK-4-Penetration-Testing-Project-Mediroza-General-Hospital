# 🏥 Mediroza General Hospital — Penetration Testing Report

##  Project Overview

As part of the **NetworkWalks Cybersecurity Internship — Batch B082, Week 4**, I conducted a **3-day black-box penetration testing assessment** of a simulated public-facing healthcare web application.

The assessment focused on identifying security vulnerabilities, validating their real-world impact, and providing practical remediation recommendations.

>  **Disclaimer:** This assessment was conducted in a controlled environment for educational purposes and under written authorization. No testing should ever be performed against systems without explicit permission.

---

## Objectives

* Identify vulnerabilities in the public-facing web application
* Assess authentication and access-control mechanisms
* Identify exposed sensitive information
* Demonstrate the impact of discovered vulnerabilities
* Provide actionable remediation recommendations
* Document findings using a professional penetration testing methodology

---

## Tools & Technologies

* **Kali Linux**
* **curl**
* **Manual Web Browser Testing**
* **robots.txt Analysis**
* **NetworkWalks Hash Calculator**
* **NetworkWalks Password Cracker**
* **ExifTool**
* **strings**

---

## Key Vulnerabilities Identified

### 1. Authentication Bypass via SQL Injection

**Severity: Critical**

The patient portal login mechanism was vulnerable to SQL injection, allowing authentication to be bypassed without valid credentials.

This resulted in unauthorized access to restricted patient records.

### 2. Weak PDF Password Protection

**Severity: High**

Patient laboratory reports were protected using weak and predictable passwords.

Password-cracking techniques were used to demonstrate that the protection could be defeated, exposing sensitive information contained within the documents.

### 3. Sensitive Path Disclosure via robots.txt

**Severity: Low**

The `robots.txt` file disclosed restricted directories such as:

* `/patient/`
* `/staff/`
* `/old/`

Although `robots.txt` is not an access-control mechanism, this information assisted the reconnaissance process.

### 4. Exposed Database Backup & Directory Listing

**Severity: Critical**

Directory listing was enabled on restricted directories, resulting in the discovery of an exposed SQL database backup.

The backup contained sensitive staff and corporate information, demonstrating the serious impact of insecure server configuration and exposed backup files.

---

## Risk Summary

| Finding                               | Severity    |
| ------------------------------------- | ----------- |
| Authentication Bypass / SQL Injection | 🔴 Critical |
| Exposed Database Backup               | 🔴 Critical |
| Weak PDF Passwords                    | 🟠 High     |
| Sensitive Path Disclosure             | 🟡 Low      |

---

## Methodology

The assessment followed a black-box penetration testing approach:

**Reconnaissance → Enumeration → Vulnerability Identification → Exploitation → Impact Analysis → Remediation**

Testing was performed strictly within the authorized scope, with no social engineering or denial-of-service testing.

---

## Remediation Recommendations

Key recommendations included:

* Implement **parameterized queries / prepared statements**
* Conduct secure source-code reviews
* Implement rate limiting and authentication protections
* Use strong, randomly generated encryption keys
* Remove sensitive paths from `robots.txt`
* Disable unnecessary directory listing
* Remove database backups from public web directories
* Store backups outside the web root
* Encrypt sensitive backups at rest
* Implement secure data-retention and deletion policies
* Conduct regular penetration testing and vulnerability assessments

---

## Key Learning Outcomes

Through this project, I gained practical experience in:

* Web application reconnaissance
* SQL injection testing
* Authentication bypass analysis
* Directory enumeration
* Sensitive information discovery
* Password/hash analysis
* Web server misconfiguration assessment
* Risk assessment and vulnerability classification
* Penetration testing documentation
* Security remediation planning

---

## Project Documentation

The complete penetration testing report contains the detailed methodology, findings, proof of exploitation, risk ratings, and remediation recommendations.

**Author:** Anaam Umar
**Program:** NetworkWalks Cybersecurity Internship — Batch B082
**Week:** 4
**Assessment Type:** Black-box Penetration Testing
**Duration:** 3 Days
**Date:** 07 September 2026

---

### Ethical & Legal Notice

All testing described in this project was performed under written authorization and within the defined scope.

**Never perform penetration testing, vulnerability scanning, exploitation, or password-cracking activities against systems you do not own or have explicit written permission to test.**
