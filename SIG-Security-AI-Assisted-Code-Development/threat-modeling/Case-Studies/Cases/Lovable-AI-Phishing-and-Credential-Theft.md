# Jailbroken Lovable AI Used for Phishing and Credential Theft

**Key Facts:**
- Date: 4/9/2025
- Threat Actor: Compromised AI Agent
- SDLC Stage: Maintenance, Design  

**Index:**
- [Case Summary](#summary)
- [Vulnerabilities Exploited](#vulnerabilities-exploited)
- [STRIDE Threat Enumeration](#stride-threat-enumeration)
- [Recommendations for Mitigation](#recommendations-for-mitigation)
- [Control Objectives and Mitigation](#control-objectives-and-mitigations)
- [Sources](#sources)

**Related Cases:**
- [MJ Rathbun Agentic Blackmail Incident](MJ-Rathbun-Agentic-Blackmail-Incident.md) - Jailbreaking to bypass safety guardrails

---
### Summary

In April 2025, cybersecurity researchers at Guardio Labs introduced the VibeScamming Benchmark, a test measuring how easily generative AI platforms could be manipulated into building functional phishing infrastructure. They found that Lovable scored the lowest of the tools - using a staged, multi-prompt jailbreak approach, researchers got Lovable to generate a convincing fake Microsoft login page, automatically deploy it live on a URL hosted under Lovable’s own subdomain, redirect victims after their credentials were stolen, and provide a fully functional admin dashboard for reviewing captured credentials, IP addresses, timestamps, and plaintext passwords. Reported campaigns built on this technique included credential-harvesting login pages, evasion mechanisms, and real-time exfiltration of stolen data via services like Telegram and Firebase. Before this research, Proofpoint had already observed tens of thousands of Lovable URLs distributed in email-based and cryptocurrency-focused phishing campaigns, while Trend Micro documented attackers using Lovable to host fake CAPTCHA pages designed to evade automated security scanners.

### Vulnerabilities Exploited

The core vulnerability was insufficiently hardened content and safety boundaries in the underlying model, which could be bypassed through incremental, multi-turn jailbreak prompting rather than requiring any technical sophistication from the attacker. Researchers explicitly noted the platform proceeded with “no guardrails, no hesitation” once steered through a staged escalation of requests. The platform’s automated, full-stack deployment capability also enabled AI to autonomously produce, deploy, and host live, publicly reachable phishing pages under its own trusted subdomain. Built-in integration capabilities with external services meant the same automation that legitimate developers use for productive app-building could be repurposed into a credential-exfiltration pipeline. Lastly, the platform’s ease of use, free hosting tier, and credible branding lowered the barrier, so that novice threat actors with no coding skill could launch full phishing campaigns in minutes.

#### STRIDE Threat Enumeration:

- **T-2 Jailbreaking / Role-Constraint Override:** Researchers used a multi-prompt approach to steer the AI model, beginning with a direct prompt asking the tool to automate each step of the attack cycle, then progressively enhancing the phishing page and delivery methods.
- **E-4 Compliance Boundary Bypass:** Lovable's intended content-safety boundaries around generating credential-harvesting and brand-impersonation content were bypassed entirely.
- **E-3 Over-Privileged Tool Access:** The AI had tool access extended to full-stack deployment, hosting, and backend infrastructure - capability far beyond content generation.
- **I-1 Data Exfiltration via Tool Calls:** The platform’s own integration capabilities were powering the exfiltration pipeline.

### Recommendations for Mitigation

A first step here would be hardening the model’s refusal of adversarial, staged jailbreak prompting through adversarial input and intent classifiers capable of recognizing incremental escalation toward a harmful end goal, even when no single prompt in isolation looks overtly malicious. Beyond the model layer, the platform’s automated deployment pipeline needs a policy gate that screens generated output for phishing-indicative patterns before allowing anything to go live on a public, platform-hosted subdomain. High-risk categories of requests, such as generating a login page or connecting a generated app to an external messaging or webhook service, should be treated as requiring heightened scrutiny by default. On the operational side, the platform needs proactive, ongoing monitoring for abuse signatures at scale paired with a fast takedown capability. Institutionalizing continuous adversarial red-teaming would help catch and close these gaps before they’re exploited at scale in production.

#### Control Objectives and Mitigations:

- **Adversarial Input Classifier:** Screen prompts for known jailbreak patterns before generating content.
- **Intent Classifier:** Detect the underlying goal of a request, regardless of how innocuously it’s phrased or staged across multiple prompts.
- **Compliance Scope Classifier**: Flag generated content that closely mimics real, recognizable brands before allowing publication.
- **Code Quality Classifier** and **Static Analysis Veto:** Scan generated application output for phishing-indicative patterns before allowing auto-deployment.
- **Decision Controls Policy Gate** and **Risk-Tier Classifier:** Treat categories like “generate a login page,” “connect to an external data-collection service,” or “clone this existing site” as high-risk actions requiring additional scrutiny or review before automatic public deployment.
- **Global Access Controls:** Restrict what generated applications can automatically integrate with without review.
- **Detection Response Integration** and **Kill Switch:** Proactive, ongoing monitoring for abuse signatures paired with rapid takedown of identified malicious hosted pages and subdomains.
- **Red Team Exercises** and **Continuous Evaluation Suite**: Implement adversarial testing rather than relying on external researchers to surface the platform’s weak points.

### Sources

- https://incidentdatabase.ai/cite/1016/
- https://guard.io/labs/vibescamming-from-prompt-to-phish-benchmarking-popular-ai-agents-resistance-to-the-dark-side
- https://thehackernews.com/2025/04/lovable-ai-found-most-vulnerable-to.html
- https://www.proofpoint.com/us/blog/threat-insight/cybercriminals-abuse-ai-website-creation-app-phishing
