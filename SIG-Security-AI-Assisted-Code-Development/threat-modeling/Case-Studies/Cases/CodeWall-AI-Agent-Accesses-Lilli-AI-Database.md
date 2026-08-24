# CodeWall AI Agent Obtains Unauthorized Access to Lilli AI Platform Database

**Key Facts:**
- Date: 2/28/2026
- Threat Actor: External Adversary
- SDLC Stage: Development, Design 

**Index:**
- [Case Summary](#summary)
- [Vulnerabilities Exploited](#vulnerabilities-exploited)
- [STRIDE Threat Enumeration](#stride-threat-enumeration)
- [Recommendations for Mitigation](#recommendations-for-mitigation)
- [Control Objectives and Mitigation](#control-objectives-and-mitigations)
- [Sources](#sources)

**Related Cases:**
- [Cranium Discovers AI Coding Assistant Hijacking Exploit](Cranium-Discovers-AI-Coding-Assistant-Hijacking-Exploit.md) - Prompt injection via trusted/passive content
- [Malicious Wiping Command in Amazon Q AI Assistant](Malicious-Wiping-Command-in-Amazon-Q-AI-Assistant.md) - Credential/token over-scoping enabling escalation
- [PocketOS Production Database Wipe](PocketOS-Production-Database-Wipe.md) - Credential/token over-scoping enabling escalation

---
### Summary

On February 28, 2026, security firm CodeWall pointed an autonomous offensive AI agent at McKinsey & Company’s internal AI platform Lilli, a system used by more than 43,000 employees for chat, document analysis, and retrieval-augmented search. The agent had no credentials, no insider knowledge, and no human-in-the-loop guidance after target selection. Within two hours, the agent had obtained full read and write access to Lilli’s production database, encompassing 46.5 million chat messages, 728,000 files, 57,000 user accounts, and 95 system prompts. The entry point was a publicly documented, unauthenticated endpoint with an SQL injection vulnerability, accepting user queries and writing them to the database without input validation. The compromised database also contained the 95 system prompts controlling how the AI reasoned, enforced guardrails, and decided what to refuse, with no access restriction beyond the authentication the agent had already bypassed. McKinsey’s security team received CodeWall’s disclosure on March 1, and the CISO acknowledged and patched the unauthenticated endpoints within a day, later stating publicly that no evidence was found of client data being accessed by unauthorized parties.

### Vulnerabilities Exploited

Foundational was a classic SQL injection vulnerability in an unauthenticated API endpoint. This was compounded by an authorization design failure: Many of Lilli’s endpoints required no authentication at all, meaning any caller, human or automated, could reach functionality that should have been gated behind identity verification. The most consequential aspect, however, was AI-specific. The system prompts governing the AI’s reasoning, guardrails, and refusal behavior were stored in the same general-purpose production database as ordinary chat and file data, with no dedicated protection separating governance material from user content. That meant an attacker with write access could have silently rewritten the instructions shaping every consultant’s AI-generated advice. Such tampering would trigger none of the usual alarms as no files would visibly change and no processes would behave abnormally, leaving the AI’s outputs subtly and undetectably compromised.

#### STRIDE Threat Enumeration:

- **T-3 Compromised Internal Data Source:** The internal production database backing Lilli’s RAG and chat functionality was directly compromised through the SQL injection vulnerability.
- **T-4 Governance Artifact Tampering:** The documents controlling AI behavior for tens of thousands of users sat unprotected inside a generic application database.
- **I-3 Over-Broad Context Exposure:** A single compromised endpoint exposed the entirety of Lilli’s data, with no segmentation limiting the blast radius.
- **E-3 Over-Privileged Tool Access:** 22 endpoints required no authentication at all, allowing the AI agent to read and write over production data without identity verification.
- **R-1 Audit Trail Gaps:** Standard monitoring had no mechanism to flag tampering with governance artifacts specifically.

### Recommendations for Mitigation

Standard application security fundamentals need to be non-negotiable: parameterized queries and input validation to close off injection vectors, and deny-by-default authentication on every endpoint capable of touching production data. The incident also points to a distinct requirement for AI platforms. Governance artifacts such as system prompts need to be treated as protected, isolated assets with their own access controls and change management, never stored alongside general application data where a single compromised endpoint can expose both simultaneously. Monitoring needs to evolve to catch this new failure mode, as prompt tampering produces none of the traditional signals of a breach. Instead, organizations running AI platforms need behavior-level auditing specifically designed to detect changes to the instructions governing model behavior. Finally, McKinsey’s own vulnerability scanners missed these flaws for over two years, making a strong argument for continuous, automated adversarial testing of production AI systems as a standing practice.

#### Control Objectives and Mitigations:

- **Test and Invariant Gate and Static Analysis Veto:** Proper input validation and parameterized queries catch SQL injection before deployment.
- **Least-Privilege Tool Access:** Implement deny-by-default authentication on every endpoint.
- **Protected Invariants:** System prompts and other AI governance artifacts should never live in the same unrestricted database as general application data. They need isolation, dedicated access controls, and change management separate from ordinary user content - this also applies at platform scale.
- **Service Hardening:** Minimize the publicly exposed attack surface.
- **Tamper-Evident Decision Logging:** Need for behavior-level auditing tuned to detect changes to governance artifacts.
- **Adversarial Change Pipeline and Red Team Exercises:** Continuous, automated adversarial testing should be a standard practice for production AI platforms.
- **Continuous Evaluation Suite:** Ongoing, automated security evaluation of live AI systems rather than periodic, point-in-time assessments.

### Sources

- https://incidentdatabase.ai/cite/1412/
- https://codewall.ai/blog/how-we-hacked-mckinseys-ai-platform 
- https://www.mindstudio.ai/blog/mckinsey-lily-ai-platform-hacked-20-dollars-6-enterprise-ai-security-failures
- https://treblle.com/blog/codewall-hack-mckinsey-ai-platform-lilli
- https://neuraltrust.ai/blog/agent-hacked-mckinsey