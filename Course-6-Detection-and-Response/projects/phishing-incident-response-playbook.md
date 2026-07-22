# Phishing Incident Response — Applying a Playbook

## Project Description

In this activity, I acted as a **Level 1 SOC analyst** at a financial services company, responding to a phishing alert by following my organization's **incident response playbook**. Building on a previous investigation that verified a file attachment as malicious, I evaluated the full alert ticket, worked through the playbook's decision flowchart, and made the escalation decision  then documented my findings and reasoning directly in the alert ticket.

A playbook is a set of step-by-step instructions that guides an analyst through responding to a specific type of incident. Playbooks matter because incident response demands coordinated, fast, consistent action: they reduce response time, minimise impact, and ensure every analyst handles the same situation the same defensible way.

---

## Scenario

```
Role:    Level 1 SOC Analyst, financial services company
Alert:   A-2703 — SERVER-MAIL Phishing attempt, possible malware download
Task:    Follow the phishing playbook to investigate and resolve the ticket
```

The attachment's SHA256 hash had already been verified as malicious in a prior investigation (identified as the **Flagpro** malware, associated with the **BlackTech** APT actor). My job here was to complete the process evaluate, decide, and resolve.

---

## The Alert Ticket

```
Ticket ID:  A-2703
Alert:      SERVER-MAIL Phishing attempt, possible download of malware
Severity:   Medium
Details:    The user may have opened a malicious email and opened
            attachments or clicked links.
Status:     Open → (my job to resolve)
```

**The email under investigation:**
```
From:     Def Communications <76tguyhh6tgftrt7tg.su>  <114.114.114.114>
To:       hr@inergy.com  <176.157.125.93>
Sent:     Wednesday, July 20, 2022 09:30:14 AM
Subject:  Re: Infrastructure Egnieer role
Body:     A job-application message asking HR to open an attached
          "resume", password-protected, with the password included.
Attachment: bfsvc.exe
Known malicious hash:
  54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b
```

---

## Step 1 — Evaluate the Alert (the 5 W's)

Before deciding anything, I gathered the incident into the 5 W's:

| | |
|---|---|
| **Who** | An external attacker posing as a job applicant, "Clyde West" from "Def Communications". The verified malware (Flagpro) is linked to the BlackTech threat actor. |
| **What** | A phishing email delivered a malicious executable (`bfsvc.exe`) disguised as a password-protected resume. The employee opened it, executing a malicious payload. |
| **When** | The email was sent Wednesday, 20 July 2022, 09:30 AM, and was received and opened by the HR employee. |
| **Where** | The HR department of the organization (`hr@inergy.com`); the affected asset is the employee's workstation. |
| **Why** | To gain access to the company's systems deceiving an employee into executing malware that establishes a foothold for the attacker. |

---

## Step 2 — Analysing the Red Flags

Evaluating the ticket surfaced multiple independent indicators that this is a genuine phishing attack:

```
🚩 Verified malicious attachment
   → hash confirmed malicious by 50+ vendors (Flagpro / BlackTech)

🚩 Sender name/address mismatch
   → "Def Communications" vs 76tguyhh6tgftrt7tg.su (gibberish address)

🚩 Suspicious top-level domain
   → .su (former Soviet Union) is heavily abused for malware/spam

🚩 Grammatical errors
   → "Infrastructure Egnieer" (subject), "writing for to express",
     "There is attached my resume" — classic phishing tell

🚩 Password-protected attachment + password in the email
   → encryption hides the file from email security scanners;
     the user is handed the key to unlock it themselves

🚩 Executable disguised as a resume
   → the attachment is bfsvc.exe — a program, not a PDF/DOC.
     No legitimate applicant sends a resume as an .exe
```

Any one of these would raise suspicion. Together, they make the verdict unambiguous.

---

## Step 3 — Working the Playbook Flowchart

The playbook drove the decision through a clear branching path:

```
Step 2: Evaluate the alert
        ↓
Step 3.0: Does the email contain links or attachments?
        → YES  (bfsvc.exe)
        ↓
Step 3.1: Are the links or attachments malicious?
        → YES  (hash verified malicious via VirusTotal)
        ↓
Step 3.2: Update the ticket and ESCALATE
        → notify a Level 2 SOC analyst
```

Had either answer been "no", the path would have led to **Step 4: Close the ticket**. But with a confirmed-malicious attachment, the playbook routes decisively to **escalation**.

**A note on the severity field:** the alert is rated *Medium*, and the playbook states that Medium severity *may* require escalation. The deciding factor wasn't the severity label alone  it was the confirmed-malicious attachment, which removes any ambiguity.

---

## Step 4 — Resolution

**Decision: ESCALATE to a Level 2 SOC analyst.**

I updated the ticket status to **Escalated** and documented the reasoning in the ticket comments:

```
Reason 1 — Confirmed malicious attachment
  bfsvc.exe's SHA256 hash was flagged by 50+ vendors on VirusTotal
  and identified as Flagpro malware (BlackTech threat actor).

Reason 2 — Evasion technique + wrong file type
  A password-protected .exe disguised as a resume, with the password
  in the email, is a known method to bypass email scanning. An .exe
  is not a legitimate job-application format.

Reason 3 — Clear phishing indicators in sender details
  Sender name/address mismatch and multiple grammatical errors in
  the subject and body.
```

Because this is a Level 1 role, escalation is the correct action  the malware is confirmed and active on an employee's device, which requires Level 2 containment and remediation. My job was to investigate, decide correctly per the playbook, and hand off with a clear, evidence-backed summary.

---

## Why This Matters for Security

Playbooks are the backbone of consistent incident response. This activity demonstrates the daily reality of a SOC analyst:

```
Alert arrives  →  evaluate against the playbook  →  gather evidence
                        ↓
        follow the decision flowchart to a defensible outcome
                        ↓
        document findings clearly  →  escalate or close
```

Three things make this response effective:

**Consistency.** By following the playbook, any analyst reaches the same defensible decision  no guesswork, no missed steps.

**Documentation.** The ticket comments give the Level 2 analyst everything they need to act immediately: what happened, why it's malicious, and the specific evidence.

**Knowing your role.** A Level 1 analyst's job is triage and escalation, not containment. Escalating with a clean handoff *is* the correct outcome recognising that boundary is part of the skill.

---

## Key Concepts Reinforced

**Playbooks & flowcharts.** Step-by-step guides that ensure fast, consistent, defensible incident response.

**The 5 W's.** A structured way to capture an incident completely before acting.

**Phishing indicators.** Sender mismatches, suspicious domains, grammatical errors, and dangerous attachment types are the tells analysts learn to spot instantly.

**Encryption as an evasion tool.** A password-protected attachment is encrypted  which blinds email scanners. The same technology that protects data also helps attackers hide payloads.

**Escalation discipline.** Knowing when to escalate versus close, and handing off with complete documentation.

---

## Skills Demonstrated

```
✅ Following an incident response playbook and flowchart
✅ Evaluating a phishing alert ticket
✅ Applying the 5 W's to an incident
✅ Identifying phishing indicators (sender, domain, grammar, attachment)
✅ Recognising encryption-based evasion techniques
✅ Making an evidence-based escalation decision
✅ Documenting findings clearly for handoff to Level 2
✅ Understanding the Level 1 SOC analyst role and its boundaries
✅ Connecting threat intelligence (hash → malware → actor) to response
```

---

## References

- NIST — [SP 800-61 Rev. 2: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
- CISA — [Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/news-events/news/avoiding-social-engineering-and-phishing-attacks)
- VirusTotal — [Official Website](https://www.virustotal.com)
- MITRE — [ATT&CK: Phishing (T1566)](https://attack.mitre.org/techniques/T1566/)
