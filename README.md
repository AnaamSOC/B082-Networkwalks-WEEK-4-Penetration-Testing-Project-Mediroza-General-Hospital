# B082-Networkwalks-WEEK-4-Penetration-Testing-Project-Mediroza-General-Hospital
Black-box penetration test of a simulated hospital web app — chained SQL injection, weak PDF encryption, and exposed server misconfigurations to demonstrate full compromise of patient and staff data.
# 🏥 Mediroza General Hospital — Penetration Testing Report

![Status](https://img.shields.io/badge/Engagement-Completed-brightgreen)
![Severity](https://img.shields.io/badge/Critical%20Findings-2-red)
![Type](https://img.shields.io/badge/Type-Black--box%20Pentest-blue)
![Program](https://img.shields.io/badge/NetworkWalks-Cybersecurity%20Internship-informational)

A black-box penetration testing engagement conducted against Mediroza General Hospital's public-facing web application, completed as part of the **NetworkWalks Cybersecurity Internship Program** (Batch B082, Week 4), under the supervision of [Mr. Waqas Karim](https://www.linkedin.com/in/waqaskarim/).

> ⚠️ **Disclaimer:** This project was conducted in a fully authorised, controlled training environment for educational purposes only. All techniques documented here were performed against a designated training target with written permission. **Never apply these techniques to any system without explicit written authorisation from the owner.**

---

## Project Overview

| | |
|---|---|
| **Client** | Mediroza General Hospital |
| **Target** | `https://medirozahospital.com` |
| **Engagement Type** | Black-box Penetration Test |
| **Duration** | 3 Days |
| **Prepared By** | Anaam Umar |
| **Supervised By** | Mr. Waqas Karim, NetworkWalks |
| **Program** | NetworkWalks Cybersecurity Internship — Batch B082, Week 4 |

 **[Full Penetration Testing Report (PDF)](./Mediroza_Pentest_Report_M4.pdf)**

---

## Objective

Identify exploitable vulnerabilities in the target's web infrastructure, demonstrate real-world impact through controlled exploitation, and document all findings in a professional penetration testing report — following the same standards expected of a real-world engagement.

---

## Methodology

The engagement followed a structured black-box methodology across 4 milestones:

1. **M1 — Initial Access:** Reconnaissance, enumeration, and exploitation of the patient portal to retrieve confidential patient records.
2. **M2 — Data Extraction:** Cracking the encryption protecting the retrieved patient PDF files.
3. **M3 — Critical Exposure Discovery:** Identifying a further, more severe exposure on the server.
4. **M4 — Reporting:** Professional documentation of all findings, risk ratings, and remediation guidance.

**Tools used:** Kali Linux, `curl`, Gobuster, manual browser-based testing, `robots.txt` analysis, [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/), [NetworkWalks Password Cracker](https://networkwalks.com/password-cracker/), `exiftool`/`strings`.

---

## Summary of Findings

| # | Finding | Milestone | Severity |
|---|---|---|---|
| 1 | Authentication Bypass via SQL Injection (Patient Portal Login) | M1 | 🔴 Critical |
| 2 | Weak / Crackable Encryption on Retrieved PDF Files | M2 | 🟠 High |
| 3 | Sensitive Path Disclosure via `robots.txt` | M3 | 🟡 Low |
| 4 | Exposed Directory Listings & Unauthenticated Database Backup | M3 | 🔴 Critical |

---

## Finding 1 — Authentication Bypass via SQL Injection (M1)

**Severity:** Critical &nbsp;|&nbsp; **Component:** `/patient/login.php`

The patient portal login form was vulnerable to SQL injection, allowing complete authentication bypass without valid credentials — granting unauthenticated access to the restricted patient area and its confidential lab reports.

**Proof of Exploitation:**

| Step | Evidence |
|---|---|
| Initial reconnaissance (`curl -I`) | ![Recon](./screenshots/M1-01-initial-recon.png) |
| Located patient portal login page | ![Login Page](./screenshots/M1-02-patient-login-page.png) |
| Baseline test with invalid credentials | ![Normal Login Test](./screenshots/M1-03-normal-login-test.png) |
| SQL injection payload triggers DB error | ![SQLi Test](./screenshots/M1-04-sqli-test.png) |
| Authentication bypassed — portal & 3 reports exposed | ![Auth Bypass](./screenshots/M1-05-auth-bypass-portal-reports.png) |
| Retrieved all 3 confidential PDF lab reports (password-protected) | ![Password Prompt 1](./screenshots/M1-06-password-prompt-report1.png) ![Password Prompt 2](./screenshots/M1-07-password-prompt-report2.png) ![Password Prompt 3](./screenshots/M1-08-password-prompt-report3.png) |

**Impact:** An unauthenticated attacker can access any patient's confidential medical records — a complete breach of patient confidentiality.

---

## Finding 2 — Weak Encryption on Retrieved PDF Files (M2)

**Severity:** High &nbsp;|&nbsp; **Component:** `Patient_Report_1/2/3.pdf`

Each retrieved PDF was password-protected, but all three used weak, guessable passwords that were recovered via hash extraction and password cracking.

**Proof of Exploitation:**

| Step | Evidence |
|---|---|
| Hash extracted — Report 1 | ![Hash 1](./screenshots/M2-01-hash-generated-report1.png) |
| Hash extracted — Report 2 | ![Hash 2](./screenshots/M2-02-hash-generated-report2.png) |
| Hash extracted — Report 3 | ![Hash 3](./screenshots/M2-03-hash-generated-report3.png) |
| Password cracked (sample 1) | ![Cracked A](./screenshots/M2-04-password-cracked-a.jpg) |
| Password cracked (sample 2) | ![Cracked B](./screenshots/M2-05-password-cracked-b.jpg) |
| Password cracked (sample 3) | ![Cracked C](./screenshots/M2-06-password-cracked-c.jpg) |
| Decrypted Report 1 | ![Decrypted 1](./screenshots/M2-07-decrypted-report1.png) |
| Decrypted Report 2 | ![Decrypted 2](./screenshots/M2-08-decrypted-report2.png) |
| Decrypted Report 3 | ![Decrypted 3](./screenshots/M2-09-decrypted-report3.png) |

**Impact:** Weak passwords provide only superficial protection — once a file is obtained, its contents can be trivially recovered using freely available tools.

---

## Finding 3 — Sensitive Path Disclosure via `robots.txt` (M3)

**Severity:** Low &nbsp;|&nbsp; **Component:** `/robots.txt`

`robots.txt` explicitly listed sensitive directory names (`/patient/`, `/staff/`, `/old/`), directly assisting in the discovery of Finding 4.

![Robots.txt](./screenshots/M3-01-robots-txt.png)

---

## Finding 4 — Exposed Database Backup via Directory Listing (M3)

**Severity:** Critical &nbsp;|&nbsp; **Component:** `/staff/`, `/old/`, `/old/mediroza_db_backup_2019.sql`

The `/staff/` and `/old/` directories had directory listing enabled with no access control, exposing a full, unauthenticated SQL database backup containing internal HR and shareholder data.

**Proof of Exploitation:**

| Step | Evidence |
|---|---|
| Open directory listings on `/staff/` and `/old/` | ![Directory Listing](./screenshots/M3-02-staff-old-directory-listing.png) |
| Database backup downloaded, unauthenticated | ![DB Downloaded](./screenshots/M3-03-db-backup-downloaded.png) |
| Staff table — 30 employees, salaries, national IDs | ![Staff Table](./screenshots/M3-04-staff-table.png) |
| Shareholders table — full equity register | ![Shareholders Table](./screenshots/M3-05-shareholders-table.png) |
| Due diligence: `/patient/reports/` correctly returns 403 | ![403 Reports](./screenshots/M3-06-patient-reports-403.png) |
| Due diligence: `/patient/error_log` correctly returns 403 | ![403 Error Log](./screenshots/M3-07-error-log-403.png) |

**Impact:** A single unauthenticated request exposed national ID numbers, salaries, and confidential shareholder equity data — a severe identity-theft, fraud, and reputational risk.

---

## Risk Rating Summary

| Finding | Likelihood | Impact | Overall Risk |
|---|---|---|---|
| SQL Injection — Auth Bypass | High | Critical | 🔴 **Critical** |
| Weak/Crackable PDF Passwords | High | High | 🟠 **High** |
| `robots.txt` Disclosure | High | Low | 🟡 **Low** |
| Exposed DB Backup | High | Critical | 🔴 **Critical** |

---

## Key Recommendations

- Use parameterised queries / prepared statements — never concatenate user input into SQL.
- Enforce strong, randomly generated passwords/keys for any document encryption.
- Never rely on `robots.txt` for access control — enforce restrictions server-side.
- Disable directory listing (autoindexing) server-wide.
- Store database backups outside the web root, encrypted, with strict access control.
- Conduct regular penetration testing and implement centralised security monitoring.

*Full remediation details for each finding are available in the [complete report](./Mediroza_Pentest_Report_M4.pdf).*

---

## Repository Structure

```
├── README.md
├── Mediroza_Pentest_Report_M4.pdf      # Full detailed report
└── screenshots/                         # All proof-of-exploitation evidence
    ├── M1-01-initial-recon.png
    ├── M1-02-patient-login-page.png
    ├── ...
    └── M3-07-error-log-403.png
```

---

## Acknowledgements

This project was completed as part of the **NetworkWalks Cybersecurity Internship Program**, under the guidance and supervision of **[Mr. Waqas Karim](https://www.linkedin.com/in/waqaskarim/)**.

---

*This engagement was conducted in a controlled environment for educational purposes only, under written authorisation from the client. These techniques must never be applied to any system without explicit written permission from the owner.*
