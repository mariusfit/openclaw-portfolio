# Premium Prompt: Advanced Security Audit & Compliance Framework Generator

**Type:** Security/Compliance/DevOps  
**Price:** $6.99  
**Difficulty:** Advanced  
**Est. Time to Value:** 4-6 weeks  
**Target Audience:** CISO, Security Engineers, Compliance Officers, DevOps Leads  

---

## 📋 Description

A comprehensive security audit framework generator that creates organization-specific security assessments, compliance mappings, and remediation roadmaps. This prompt guides you through building a complete security audit program tailored to your industry, infrastructure, and risk profile—replacing weeks of security consulting.

**Use Cases:**
- Enterprises preparing for SOC 2 Type II or ISO 27001 certification
- Mid-market companies establishing security baselines post-acquisition
- DevOps teams needing to audit cloud infrastructure (AWS/Azure/GCP)
- Startups building compliance from day one
- Security teams conducting internal risk assessments

---

## 🎯 Prompt

```
You are an expert information security architect and compliance framework designer.

Your task: Create a comprehensive security audit framework for [ORGANIZATION_NAME] based on the following inputs:

## ORGANIZATION CONTEXT
- **Industry:** [E.g., Healthcare, FinTech, E-commerce, SaaS]
- **Company size:** [E.g., 50 employees, $10M ARR]
- **Infrastructure:** [E.g., AWS multi-region, on-premises Kubernetes, hybrid]
- **Current stage:** [E.g., Series A, pre-IPO, regulated enterprise]
- **Compliance requirements:** [E.g., HIPAA, PCI-DSS, SOC 2, GDPR, none yet]
- **Existing security tools:** [E.g., Okta, CrowdStrike, GuardDuty, etc. or none]
- **Known risks or prior incidents:** [Describe any known vulnerabilities or past breaches]

## OUTPUT REQUIREMENTS
Generate a complete audit framework with these components:

### 1. AUDIT SCOPE & DIMENSIONS
- **7 core pillars** aligned to your industry (e.g., for SaaS: IAM, Data Protection, Infrastructure, Application Security, Incident Response, Vendor Management, Compliance)
- **3 assessment levels** (foundational, intermediate, advanced)
- **Risk matrix** mapping controls to business impact (Confidentiality, Integrity, Availability)

### 2. CONTROL FRAMEWORK
- **50+ detailed controls** organized by pillar
- **For each control:**
  - Control ID (e.g., IAM-001)
  - Control title
  - Risk category (preventive/detective/responsive)
  - Compliance mapping (which standards cover this?)
  - Implementation difficulty (1-5, with effort estimate)
  - Maturity level (1=manual, 5=automated + AI-driven)
  - Success metrics (how do you know it's working?)
  - Quick-win checks (5-minute tests)
  - Advanced checks (tools + commands)

### 3. AUDIT CHECKLIST (EXECUTABLE)
- **Checklist format** (markdown + JSON)
- **Quick-scan** (1 hour, 20 critical controls)
- **Standard audit** (1 week, 50 controls)
- **Deep audit** (1 month, 50+ controls + manual testing)
- **Automation playbook:** bash, Python, or Terraform scripts to auto-check controls
- **Each line:** question, how to validate, pass/fail criteria, evidence format

### 4. REMEDIATION ROADMAP
- **Priority tiers:**
  - 🔴 **Critical** (fix in 24-48h, will cause incidents if not)
  - 🟠 **High** (fix in 2 weeks, creates material risk)
  - 🟡 **Medium** (fix in 1-3 months, best practices gap)
  - 🟢 **Low** (fix in quarter, nice-to-have)
- **For each priority tier:** risk statement, business justification, implementation steps, effort (days), cost (tooling + labor), success criteria
- **12-month roadmap:** phased plan with dependencies and milestones

### 5. TOOL & AUTOMATION RECOMMENDATIONS
- **Audit tools** (e.g., Prowler for AWS, kube-bench for Kubernetes)
- **SIEM/Logging:** recommended stack for your scale
- **Threat modeling:** top 5 attack scenarios relevant to your industry
- **Automation:** which controls can be auto-checked monthly/weekly/daily?
- **Integration:** how to fold audit findings into your incident response and change management

### 6. METRICS & REPORTING
- **Security posture score** (0-100, based on control maturity)
- **Trend tracking:** quarterly comparisons
- **Board-level dashboard** (1-page summary for execs)
- **Team dashboard** (detailed, for security/ops teams)
- **Audit report template** (executive summary + detailed findings + roadmap)

### 7. COMPLIANCE MAPPING TABLE
- **Row:** each control (from Framework)
- **Columns:** NIST, ISO 27001, SOC 2, HIPAA, PCI-DSS, GDPR (where applicable)
- **Mark:** which controls satisfy which standards
- **Gaps:** highlight missing controls for your target certifications

### 8. INCIDENT RESPONSE INTEGRATION
- **How audit findings inform IR:** top 5 attack scenarios based on audit results
- **Post-breach audit:** 10-step forensic audit plan if incident occurs
- **Lessons learned:** audit template for post-mortems

---

## FORMATTING & DEPTH REQUIREMENTS

✅ **DO:**
- Provide scripts (bash, Python, Terraform) that can actually run—not pseudocode
- Include real CLI commands (aws, kubectl, curl, openssl, dig, etc.)
- Write pass/fail criteria in boolean logic (if X then pass, else fail)
- Reference real tools (Prowler, kube-bench, Trivy, Falco, etc.)
- Add real-world time estimates (not "a few hours")
- Use industry jargon correctly but explain once

❌ **DON'T:**
- Generic templates ("check firewalls")
- Unspecified effort estimates
- Controls that aren't testable
- Compliance mapping without justification
- Automation scripts that won't run

---

## TONE & PERSPECTIVE
- **Authority:** You are an experienced security auditor who has done this 50+ times
- **Pragmatism:** Balance perfect security with business reality (acknowledge trade-offs)
- **Actionability:** Every recommendation includes a "start today" task
- **Realism:** Estimate effort honestly; highlight quick wins vs. hard problems
- **Risk awareness:** Always tie controls back to business impact, not just compliance

---

## EXAMPLE OUTPUT STRUCTURE (for an AWS SaaS startup)

**Audit Framework: TechCorp Inc. (Series B SaaS, AWS, 80 employees, targeting SOC 2 Type II in 18 months)**

### 1. Audit Dimensions
**Pillars:** IAM & Access, Data & Encryption, Infrastructure Hardening, Application Security, Incident Response, Vendor Management, Compliance & Governance

**Levels:** Foundation (16 controls), Intermediate (18 controls), Advanced (16 controls) = 50 total

### 2. Sample Control (IAM-003: MFA Enforcement)
- **Risk:** Compromised credentials lead to account takeover
- **Compliance:** NIST AC-2, SOC 2 Criterion 1.4.1, ISO 27001 A.6.2.1
- **Effort:** 2 days (AWS Cognito config + user comms)
- **Maturity:** L1 → L3 (manual → Okta-enforced)
- **Quick check (5 min):**
  ```bash
  aws iam get-account-summary | grep -i "mfa"
  ```
- **Pass:** All human users have MFA enabled; all workloads use temporary credentials
- **Evidence:** IAM user list with MFA column; workload role audit

### 3. Audit Checklist: Quick-Scan (1 hour)
1. ☐ All AWS IAM users have MFA (script: `aws iam list-virtual-mfa-devices`)
2. ☐ Root account MFA enabled & not used for daily tasks (script: `aws iam list-mfa-devices`)
3. ☐ All S3 buckets block public access (script: `aws s3api list-buckets && aws s3api get-bucket-acl`)
4. ☐ CloudTrail enabled on all regions (script: `aws cloudtrail describe-trails`)
5. ☐ VPC Flow Logs enabled (script: `aws ec2 describe-flow-logs`)
[... 15 more checks with scripts]

### 4. Remediation Roadmap
**🔴 CRITICAL (2 weeks):**
- MFA enforcement: 2 days, $0 (AWS built-in)
- S3 bucket encryption: 1 day, $0
- CloudTrail centralization: 3 days, $200/mo

**🟠 HIGH (4 weeks):**
- Secrets manager rotation: 1 week, $0.4/mo
- VPC security groups audit: 3 days, $0

[... etc.]

---

Now, generate the complete audit framework for [ORGANIZATION_NAME] using the inputs above. Be specific, actionable, and reference real tools and scripts. The output should be usable as a checklist for next week's audit kickoff.
```

---

## 💡 Real-World Value

This prompt replaces a **2-week security consulting engagement ($8K-15K)** or **1 full-time security engineer for 4-6 weeks.**

**Buyers get:**
- Audit checklist they can run immediately
- Compliance mapping for their specific requirements
- 50+ controls with implementation guidance
- Automation scripts (copy-paste ready)
- 12-month roadmap with business case for each control
- Board-ready reporting templates

**Typical workflow:**
1. Fill in org context (30 min)
2. Run audit framework output (1-2 weeks)
3. Create remediation tickets from roadmap
4. Show progress to board/investors quarterly

---

## 🎯 Suggested Tags (for PromptBase)
- security audit
- compliance framework
- SOC 2
- ISO 27001
- HIPAA
- DevOps security
- cloud security
- AWS
- Kubernetes
- compliance automation
- CIS benchmarks

---

## 📊 Competitive Analysis

**Similar products:**
- Prowler reports ($0, but outputs raw findings, not roadmaps)
- Ermetic/Wiz ($30K+/year, tool-based, not customizable)
- Security consulting ($8K-15K/week)

**Unique value:** Custom, actionable, industry-specific, with automation + roadmap. This prompt is **not** just a generic checklist—it's a real engagement plan.

---

**Last Updated:** 2026-02-24  
**Estimated Effort to Create:** ~8 hours per org (after first template built)  
**Suggested Retail Price Range:** $5.99-8.99 (high technical depth commands premium)  
**Confidence Level:** ⭐⭐⭐⭐⭐ (proven demand in security consulting market)
