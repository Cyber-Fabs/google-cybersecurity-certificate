# Risk Register — Commercial Bank Security Risk Assessment

## Project Description

In this project, I conducted a formal risk assessment for a commercial bank by evaluating five critical vulnerabilities threatening the bank's primary asset,its funds. Using the NIST Cybersecurity Framework (CSF) as a foundation, I created a risk register to identify, score, and prioritize potential security risks based on their likelihood of occurrence and severity of impact. The risk scoring methodology applied the standard formula: **Likelihood × Impact Severity = Risk Priority**. This exercise mirrors the risk assessment process performed by GRC analysts and security teams in financial institutions to ensure regulatory compliance and protect sensitive customer data.

---

## Operational Environment

```
Location:       Coastal area with low crime rates
Employees:      100 on-premise + 20 remote = 120 total
Customer base:  2,000 individual accounts
                200 commercial accounts
Partners:       1 professional sports team
                10 local businesses
Regulations:    Strict financial regulations including
                Federal Reserve fund availability requirements
Key asset:      Bank funds and customer financial data
```

---

## Notes — Risk Factors in the Operating Environment

The bank's operational environment presents several factors that increase its exposure to security risks. With 120 employees including 20 working remotely and over 2,200 customer accounts, the large number of people and systems accessing sensitive financial data significantly increases the attack surface for social engineering attacks such as business email compromise and database breaches. The bank's coastal location introduces the additional risk of natural disasters disrupting supply chains, while strict Federal Reserve financial regulations mean that any security incident affecting fund availability or data confidentiality could result in severe regulatory penalties, reputational damage, and direct financial loss to the institution and its customers.

---

## Risk Register

| Asset | Risk | Description | Likelihood | Severity | Priority |
|-------|------|-------------|------------|----------|----------|
| Funds | Business email compromise | An employee is tricked into sharing confidential information. | 2 | 3 | 6 |
| Funds | Compromised user database | Customer data is poorly encrypted. | 2 | 3 | 6 |
| Funds | Financial records leak | A database server of backed up data is publicly accessible. | 3 | 3 | 9 |
| Funds | Theft | The bank's safe is left unlocked. | 1 | 3 | 3 |
| Funds | Supply chain disruption | Delivery delays due to natural disasters. | 1 | 2 | 2 |

---

## Risk Matrix Reference

|  | Low (1) | Moderate (2) | Catastrophic (3) |
|--|---------|--------------|-----------------|
| **Certain (3)** | 3 | 6 | 9 |
| **Likely (2)** | 2 | 4 | 6 |
| **Rare (1)** | 1 | 2 | 3 |

---

## Risk Scoring Justification

### 1 — Business Email Compromise
**Likelihood: 2 (Moderate) | Severity: 3 (Catastrophic) | Priority: 6**

```
Likelihood = 2 (Moderate)
→ The bank has 120 employees including 20 remote workers
→ Remote employees are particularly vulnerable to phishing
→ Business email compromise is the most common form
  of cybercrime targeting financial institutions
→ With multiple employees handling sensitive data daily
  the probability of at least one successful social
  engineering attempt is moderate
→ Low local crime rate does not reduce online threat actors

Severity = 3 (Catastrophic)
→ An employee tricked into sharing confidential information
  could expose customer financial data
→ Could result in unauthorized fund transfers
→ Federal Reserve regulatory violations likely
→ Reputational damage to a bank trusted by 2,200+ customers
→ Potential loss of commercial account partnerships
→ Financial and legal consequences are severe
```

---

### 2 — Compromised User Database
**Likelihood: 2 (Moderate) | Severity: 3 (Catastrophic) | Priority: 6**

```
Likelihood = 2 (Moderate)
→ Poor encryption of customer data is a known
  and common vulnerability in financial systems
→ With 2,000 individual and 200 commercial accounts
  the database is a high value target for attackers
→ Multiple systems and employees accessing the database
  increases the exposure window
→ Not certain but a realistic and moderate probability
  given the volume of data and number of access points

Severity = 3 (Catastrophic)
→ Exposure of customer financial data violates
  strict banking regulations and privacy laws
→ 2,200+ customer accounts compromised simultaneously
→ Heavy regulatory fines from financial authorities
→ Loss of customer trust destroys bank reputation
→ Commercial account holders would likely terminate
  relationships immediately
→ Class action lawsuits from affected customers possible
```

---

### 3 — Financial Records Leak
**Likelihood: 3 (Certain) | Severity: 3 (Catastrophic) | Priority: 9**

```
Likelihood = 3 (Certain/High)
→ A publicly accessible backup database server is
  an active and exploitable vulnerability RIGHT NOW
→ Unlike theoretical risks this is a confirmed
  misconfiguration that is actively exposed
→ Automated scanning tools used by attackers would
  discover this server quickly
→ The likelihood of exploitation is near certain
  if the vulnerability is not immediately remediated
→ This is the highest priority risk in the register

Severity = 3 (Catastrophic)
→ Backed up financial records contain complete
  history of all customer transactions
→ Exposure violates Federal Reserve regulations
  and banking data protection requirements
→ Complete financial history of 2,200+ accounts exposed
→ Enables identity theft, fraud, and account takeover
→ Maximum regulatory fines and legal liability
→ Existential reputational damage to the institution
→ Professional sports team and business partners
  would immediately terminate marketing relationships
```

---

### 4 — Theft
**Likelihood: 1 (Rare) | Severity: 3 (Catastrophic) | Priority: 3**

```
Likelihood = 1 (Rare)
→ The bank is located in a coastal area with LOW crime rates
→ Physical theft of bank funds is statistically uncommon
  in low-crime environments
→ Banks have multiple physical security layers
  even with an unlocked safe scenario
→ Security cameras, guards, and alarms reduce
  the probability of successful physical theft
→ While possible the likelihood is low given
  the environmental and physical security context

Severity = 3 (Catastrophic)
→ Direct loss of physical funds impacts daily
  Federal Reserve cash availability requirements
→ Failure to meet Federal Reserve requirements
  results in immediate regulatory penalties
→ Insurance claims and investigations disrupt
  normal banking operations
→ Reputational damage if news becomes public
→ Even with low likelihood the potential impact
  is severe enough to warrant attention
```

---

### 5 — Supply Chain Disruption
**Likelihood: 1 (Rare) | Severity: 2 (Moderate) | Priority: 2**

```
Likelihood = 1 (Rare)
→ Coastal location does introduce hurricane risk
  but major hurricanes are infrequent events
→ Supply chain disruptions severe enough to impact
  the bank's fund replenishment happen rarely
→ Banks typically have contingency plans and
  alternative supply chain arrangements
→ Not impossible but low probability in any given year

Severity = 2 (Moderate)
→ Delivery delays would temporarily impact
  the bank's ability to maintain required cash levels
→ Short term disruption to Federal Reserve requirements
→ Unlike data breaches there is no permanent
  loss of customer data or funds
→ Operations can resume once supply chain is restored
→ Moderate impact compared to data compromise scenarios
→ Business continuity plans can mitigate severity
```

---

## Risk Priority Summary

| Priority | Risk | Score | Action Required |
|----------|------|-------|----------------|
| CRITICAL | Financial records leak | 9 | Immediate remediation — secure publicly accessible database server now |
| HIGH | Business email compromise | 6 | Implement email security training and MFA for all employees |
| HIGH | Compromised user database | 6 | Audit and strengthen database encryption immediately |
| MEDIUM | Theft | 3 | Review physical security protocols and safe locking procedures |
| LOW | Supply chain disruption | 2 | Develop and maintain business continuity plan for natural disasters |

---

## Recommended Security Controls

| Risk | Recommended Control | NIST CSF Function |
|------|--------------------|--------------------|
| Financial records leak | Immediately restrict public access to backup server, implement access controls, conduct full audit | PROTECT |
| Business email compromise | Security awareness training, email filtering, MFA, phishing simulations | PROTECT |
| Compromised user database | Implement AES-256 encryption, conduct encryption audit, database access controls | PROTECT |
| Theft | Physical security review, safe locking policy, security camera audit | PROTECT |
| Supply chain disruption | Business continuity plan, alternative supplier agreements, cash reserve strategy | RECOVER |

---

## NIST CSF Alignment

This risk assessment directly supports the IDENTIFY function of the NIST Cybersecurity Framework:

```
IDENTIFY → Asset Management
→ Identified key asset: Bank funds
→ Documented five risks threatening the asset
→ Assessed likelihood and severity of each risk
→ Prioritized risks for remediation

PROTECT → Risk Management Strategy
→ Recommended controls for each identified risk
→ Aligned controls to regulatory requirements
→ Federal Reserve compliance maintained

DETECT → Continuous Monitoring
→ Publicly accessible database server must be
  monitored continuously going forward
→ Email compromise attempts must be logged
  and reviewed in SIEM
```

---

## Summary

This risk assessment identified and scored five critical risks threatening the commercial bank's primary asset, its funds. Using the Likelihood x Severity = Priority formula and a 1-3 scoring scale, the financial records leak received the highest priority score of 9, representing a near-certain likelihood of exploitation and catastrophic potential impact due to an actively exposed backup database server. Business email compromise and compromised user database both scored 6, reflecting the moderate likelihood of occurrence given the large employee and customer base combined with the catastrophic regulatory and reputational consequences of a breach in a heavily regulated financial environment. Physical theft and supply chain disruption received lower priority scores of 3 and 2 respectively, reflecting the low crime rate environment and infrequent nature of natural disasters. The risk register provides the security team with a clear data-driven prioritization framework to allocate resources effectively and protect the bank's assets, customers, and regulatory standing.
