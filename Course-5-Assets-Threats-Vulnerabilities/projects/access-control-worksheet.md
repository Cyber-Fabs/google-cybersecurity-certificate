# Access Control Worksheet — Payroll Incident Investigation

## Project Description

In this project, I investigated an unauthorized payroll transaction at a growing business by analyzing event logs and cross-referencing them with the employee directory to identify the threat actor and access control failures. Using authentication and authorization analysis techniques, I identified the user responsible for the suspicious deposit, discovered critical access control vulnerabilities in the organization's security posture, and recommended specific mitigations to prevent similar incidents in the future. This exercise directly mirrors the access control auditing and incident investigation work performed daily by SOC analysts and GRC professionals in real organizations.

---

## Scenario Summary

A deposit was made from the business to an unknown bank account — FAUX_BANK. The finance manager confirmed they did not authorize the transaction. The payment was stopped in time, but an investigation was required to identify the threat actor, understand how the incident occurred, and prevent future occurrences. The investigation involved reviewing the payroll event log and cross-referencing it with the employee directory to identify access control failures.

---

## Access Controls Worksheet

| | Note(s) | Issue(s) | Recommendation(s) |
|---|---------|----------|-------------------|
| **Authorization / Authentication** | **Note 1:** The event log shows the payroll event was triggered on 10/03/2023 at 8:29:57 AM by user `Legal\Administrator` from IP address `152.207.255.255` on a computer named `Up2-NoGud`. Cross-referencing this IP address with the employee directory identifies the user as **Robert Taylor Jr.**, a Legal Attorney who worked as a contractor. **Note 2:** Robert Taylor Jr. has an end date of 43826.0 in the employee directory, indicating his contract ended before the incident date of 10/03/2023 — meaning his account should have been deactivated but remained active, allowing unauthorized access to the payroll system. | **Issue 1:** Robert Taylor Jr.'s contractor account remained active after his contract end date, violating the principle of least privilege and proper access lifecycle management. A former contractor should have had their access revoked immediately upon contract termination — not left active where it could be exploited months or years later. **Issue 2:** Every employee in the organization — regardless of role — has been granted Admin-level authorization on the shared cloud drive. A graphic designer, sales associate, and manufacturer should not have the same level of access as the owner or finance manager. This blanket Admin access means any compromised account can perform sensitive financial operations like payroll modifications. | **Recommendation 1 — Implement immediate access revocation upon offboarding:** Establish a formal offboarding procedure that automatically deactivates user accounts on their contract or employment end date. No former employee or contractor should retain active system access after their relationship with the organization ends. This should be enforced through an Identity and Access Management (IAM) system that ties account status directly to HR employment records. **Recommendation 2 — Implement Role-Based Access Control (RBAC):** Remove blanket Admin access from all employees and implement role-based permissions that grant each user only the minimum access required for their specific job function. A graphic designer needs access to design files  not payroll systems. A legal contractor needs access to legal documents not financial records. This enforces the principle of least privilege across the entire organization. |

---

## Event Log Analysis

| Field | Details | Significance |
|-------|---------|--------------|
| Event Type | Information | Payroll modification recorded |
| Event Source | AdsmEmployeeService | Payroll management system |
| Event ID | 1227 | Payroll event added |
| Date | 10/03/2023 | Date of unauthorized transaction |
| Time | 8:29:57 AM | Early morning — outside typical business hours |
| User | Legal\Administrator | Legal department admin account used |
| Computer | Up2-NoGud | Suspicious computer name — potential red flag |
| IP Address | 152.207.255.255 | Key identifier for cross-referencing |
| Description | Payroll event added. FAUX_BANK | Unauthorized bank account added to payroll |

---

## Employee Directory Cross-Reference

| Employee | Role | IP Address | Status | Authorization | Last Access | End Date |
|----------|------|-----------|--------|--------------|-------------|----------|
| Lisa Lawrence | Office manager | 118.119.20.150 | Full-time | Admin | 0 min ago | N/A |
| Jesse Pena | Graphic designer | 186.125.232.66 | Part-time | Admin | 1 day ago | N/A |
| Catherine Martin | Sales associate | 247.168.184.57 | Full-time | Admin | 10 min ago | N/A |
| Jyoti Patil | Account manager | 159.250.146.63 | Full-time | Admin | 2 hours ago | N/A |
| Joanne Phelps | Sales associate | 249.57.94.27 | Seasonal | Admin | 2 years ago | 43861.0 |
| Ariel Olson | Owner | 19.7.235.151 | Full-time | Admin | 4 min ago | N/A |
| **Robert Taylor Jr.** | **Legal attorney** | **152.207.255.255** | **Contractor** | **Admin** | **8:29:57 AM** | **43826.0** ⚠️ |
| Amanda Pearson | Manufacturer | 101.225.113.171 | Contractor | Admin | 3 months ago | N/A |
| George Harris | Security analyst | 70.188.129.105 | Full-time | Admin | 1 day ago | N/A |
| Lei Chu | Marketing | 53.49.27.117 | Part-time | Admin | 2 days ago | 43861.0 |

---

## Threat Actor Identification

```
IP Address from Event Log:  152.207.255.255
IP Address in Directory:    152.207.255.255 → Robert Taylor Jr.

MATCH CONFIRMED ✅

Threat Actor:   Robert Taylor Jr.
Role:           Legal Attorney (Contractor)
Status:         Contractor — CONTRACT EXPIRED ⚠️
End Date:       43826.0 (contract ended before incident)
Authorization:  Admin (should have been revoked) ⚠️
Computer used:  Up2-NoGud (suspicious naming)
Action taken:   Added FAUX_BANK to payroll system
Time:           8:29:57 AM on 10/03/2023
```

---

## Root Cause Analysis

### How the incident happened step by step

```
Step 1 — Robert Taylor Jr. joined as Legal Contractor
→ Given Admin access to shared cloud drive
→ Same access level as the Owner
→ No access restrictions based on role ❌

Step 2 — Contract ended (End date: 43826.0)
→ Access should have been revoked immediately
→ No offboarding procedure in place
→ Account remained fully active ❌

Step 3 — Robert Taylor Jr. accessed the system
→ Months after contract ended
→ Using still-active Admin credentials
→ From computer named "Up2-NoGud" ⚠️
→ At 8:29:57 AM on 10/03/2023

Step 4 — Unauthorized payroll modification
→ Added FAUX_BANK as payment destination
→ Admin access allowed full payroll control
→ No additional verification required ❌

Step 5 — Transaction caught in time
→ Finance manager noticed the anomaly
→ Payment stopped before completion
→ Investigation launched
```

---

## Access Control Issues Identified

### Issue 1 — No access revocation upon contract termination
```
PROBLEM:
Robert Taylor Jr.'s contractor account
remained active after his end date

IMPACT:
→ Former contractor retained full Admin access
→ Could access all company resources
→ Could modify financial systems
→ No time limit on the damage window

NIST CSF VIOLATION:
→ PR.AC-1: Identities and credentials are
  managed for authorized devices and users
→ This control was not implemented ❌

REAL WORLD EQUIVALENT:
→ Employee leaves company
→ Badge still works 6 months later
→ Can walk into any room in the building
→ This is exactly what happened digitally
```

### Issue 2 — Blanket Admin access for all employees
```
PROBLEM:
Every single employee regardless of role
has been granted Admin-level authorization

IMPACT:
→ Graphic designer has same access as Owner
→ Seasonal sales associate has Admin rights
→ Legal contractor has payroll access
→ Any compromised account = full system access
→ Massive violation of least privilege

NIST CSF VIOLATION:
→ PR.AC-4: Access permissions are managed
  incorporating the principles of least
  privilege and separation of duties
→ This control was not implemented ❌

ADDITIONAL CONCERN:
Two employees have end dates but still
appear in the directory with Admin access:
→ Joanne Phelps (Seasonal) — last access 2 years ago
→ Lei Chu (Part-time Marketing) — has end date
Both represent additional security risks
```

---

## Recommendations

### Recommendation 1 — Automated Access Revocation (IAM)

```
WHAT:
Implement an Identity and Access Management
system that automatically deactivates
accounts on their employment end date

HOW TO IMPLEMENT:
→ Connect HR system to IAM platform
→ Set automatic account deactivation
  on contract/employment end date
→ Send access revocation confirmation
  to security team on each offboarding
→ Conduct monthly audit of active accounts
  against current employee roster
→ Any account with end date in the past
  should be immediately deactivated

PREVENTS:
→ Former employees retaining access
→ Contractor account abuse after contract ends
→ Ghost accounts being exploited by attackers
→ Insider threats from disgruntled ex-employees

ADDITIONAL ACTION REQUIRED NOW:
→ Immediately deactivate Robert Taylor Jr.
→ Review and deactivate Joanne Phelps account
  (last access 2 years ago — seasonal employee)
→ Review Lei Chu account (has end date)
→ Audit ALL contractor accounts
```

---

### Recommendation 2 — Role-Based Access Control (RBAC)

```
WHAT:
Replace blanket Admin access with
role-specific permission levels based
on each employee's job function and
need-to-know requirements

HOW TO IMPLEMENT:
→ Define permission tiers:
  ADMIN    → Owner, Finance Manager only
  FINANCE  → Accounting, Payroll staff
  LEGAL    → Legal team (no financial access)
  SALES    → CRM and sales materials only
  DESIGN   → Design files and assets only
  MARKETING→ Marketing materials only
  VIEWER   → Read-only for contractors

→ Reassign all current employees:
  Lisa Lawrence (Office Manager) → Admin
  Jesse Pena (Graphic Designer)  → Design only
  Catherine Martin (Sales)       → Sales only
  Jyoti Patil (Account Manager)  → Finance
  Ariel Olson (Owner)            → Admin
  Amanda Pearson (Manufacturer)  → Viewer
  George Harris (Security)       → Admin + Audit

PREVENTS:
→ Unauthorized payroll modifications
→ Contractors accessing financial systems
→ Lateral movement after account compromise
→ Single compromised account causing
  catastrophic organizational damage
```

---

### Recommendation 3 — Multi-Factor Authentication (MFA)

```
WHAT:
Require MFA for all accounts especially
those with access to financial systems

HOW TO IMPLEMENT:
→ Enable MFA on all employee accounts
→ Require additional verification for
  any payroll or financial modifications
→ Send real-time alerts for any payroll
  changes to finance manager and owner

PREVENTS:
→ Stolen credentials being used alone
→ Unauthorized financial modifications
→ Even if password is compromised
  attacker cannot complete MFA step
```

---

## NIST CSF Alignment

```
IDENTIFY:
→ Identified threat actor through
  IP address cross-referencing
→ Identified access control gaps
  across entire employee directory

PROTECT:
→ Recommend IAM with auto-revocation
→ Recommend RBAC implementation
→ Recommend MFA for financial systems
→ These controls enforce least privilege
  and proper access management

DETECT:
→ Event log monitoring detected
  the unauthorized payroll event
→ Recommend enhanced monitoring
  for after-hours financial activity
→ Alert on payroll modifications

RESPOND:
→ Finance manager stopped payment
→ Investigation launched immediately
→ Threat actor identified

RECOVER:
→ Immediately revoke Robert Taylor Jr.
→ Audit all contractor accounts
→ Implement RBAC going forward
→ Review all payroll transactions
  for additional unauthorized activity
```

---

## Summary

This access control investigation identified Robert Taylor Jr., a former legal contractor, as the threat actor responsible for the unauthorized payroll modification to FAUX_BANK. By cross-referencing the event log IP address (152.207.255.255) with the employee directory, I confirmed his identity and discovered two critical access control failures: his contractor account remained active after his contract end date due to the absence of an offboarding and access revocation procedure, and all employees  regardless of role had been granted blanket Admin access to company resources in violation of the principle of least privilege. Implementing automated IAM-based access revocation tied to employment end dates and role-based access controls that grant each employee only the permissions their specific job requires will significantly reduce the likelihood of similar incidents occurring in the future.

---

## Skills Demonstrated

```
✅ Event log analysis and investigation
✅ IP address cross-referencing for threat identification
✅ Access control gap analysis
✅ Principle of least privilege evaluation
✅ Role-based access control design
✅ IAM policy recommendations
✅ Insider threat and contractor risk assessment
✅ NIST CSF alignment across all five functions
✅ Root cause analysis documentation
✅ Professional security incident reporting
```
