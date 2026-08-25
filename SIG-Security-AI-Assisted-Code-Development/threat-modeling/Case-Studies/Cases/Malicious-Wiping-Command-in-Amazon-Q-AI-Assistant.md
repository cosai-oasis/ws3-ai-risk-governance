# Malicious Wiping Command in Amazon Q AI Assistant
**Key Facts:**
- Date: 7/17/2026 
- Threat Actor: Compromised AI Agent, External Adversary
- SDLC Stage: Deployment, Development  

**Index:**
- [Case Summary](#summary)
- [Vulnerabilities Exploited](#vulnerabilities-exploited)
- [STRIDE Threat Enumeration](#stride-threat-enumeration)
- [Recommendations for Mitigation](#recommendations-for-mitigation)
- [Control Objectives and Mitigation](#control-objectives-and-mitigations)
- [Sources](#sources)

**Related Cases:**
- [Malicious npm Packages Weaponize AI Coding Agents](Malicious-npm-Packages-Weaponize-AI-Coding-Agents.md) - Supply chain / build-pipeline compromise
- [PocketOS Production Database Wipe](PocketOS-Production-Database-Wipe.md) - Credential/token over-scoping enabling escalation
- [CodeWall AI Agent Unauthorized Access to Lilli AI Database](CodeWall-AI-Agent-Accesses-Lilli-AI-Database.md) - Credential/token over-scoping enabling escalation

---
### Summary

In July 2025, a hacker compromised Amazon’s Q Developer Extension for Visual Studio Code by submitting a pull request to its GitHub repository that embedded a malicious prompt instructing the AI coding agent to “clean a system to a near-factory state and delete file-system and cloud resources,” using filesystem tools, bash, and the AWS CLI. The hacker embedded the prompt inside a version of Amazon Q, and that compromised version was distributed publicly through the VS Code Marketplace, where the extension had close to a million installs before the malicious code was detected and removed. Amazon stated no customer data was compromised and urged users to update to the patched version, 1.85.0.

### Vulnerabilities Exploited

The core vulnerability was that the AI agent’s behavior could be altered through a malicious prompt injected directly into source code, and that this prompt was able to pass through Amazon’s code review and merge process into a public release without being caught. In addition. the attacker appears to have leveraged compromised credentials to get the change accepted, since Amazon’s remediation explicitly included revoking compromised credentials rather than just removing the offending code. Once merged, the malicious prompt had a path to destructive, over-privileged tool access with no scoping or sandboxing in place to prevent an injected instruction from reaching that level of capability. Finally, there was a supply-chain and release-pipeline gap, as the compromised code shipped in an official, publicly distributed version (1.84.0) to nearly a million users before anyone caught it, meaning detection happened only after the vulnerable version was already in the wild rather than being caught at the review or build stage.

#### STRIDE Threat Enumeration:
- **T-1 Prompt Injection:** A malicious system prompt was embedded directly into the codebase.
- **T-6 Reviewer Infrastructure Supply Chain:** The attacker got a malicious pull request merged into the official aws-toolkit-vscode repository and shipped in a public release.
- **T-3 Compromised Internal Data Source:** The GitHub repository functioned as an internal source for the extension’s behavior - its integrity was compromised without detection at merge time.
- **S-1 Approval Credential Theft or Misuse:** The attacker likely used stolen or misused credentials to get the change approved/merged rather than relying purely on social engineering.
- **E-3 Over-Privileged Tool Access:** The agent had unrestricted filesystem, bash, and AWS CLI access sufficient to wipe local files and cloud resources.
- **E-4 Compliance Boundary Bypass:** Had the payload executed as designed, it would have bypassed all expected operational boundaries.

### Recommendations for Mitigation

Any single merged change needs to be prevented from being able to grant an AI agent destructive, unscoped access to a user’s filesystem or cloud infrastructure, which means enforcing least-privilege tool access so that even a successfully injected malicious prompt has no path to catastrophic action. This should be paired with stronger internal source integrity controls and static analysis scanning that specifically looks for destructive commands or prompt-injection patterns in code before it’s merged, along with tighter secrets management to reduce the risk of compromised credentials being used to get malicious changes approved. Multi-party review and isolated, sandboxed execution should be required for any change touching agent tool permissions or execution logic, and changes altering these code paths should be flagged for heightened scrutiny regardless of how small they appear. On the release side, supply chain controls should validate that shipped build artifacts match verified source before public distribution, and detection and response capabilities need to be fast enough to catch anomalous commits and pull a compromised release before it reaches hundreds of thousands of users. Maintaining tamper-evident logs of who approved and merged changes would also support faster root-cause analysis and help close the credential-misuse gap that appears to have enabled this in the first place.

#### Control Objectives and Mitigations:

- **Internal Source Integrity:** Verify the integrity and provenance of code merged into the repository that defines agent behavior, especially anything touching prompt content or tool-execution logic.
- **Static Analysis Veto:** Automated scanning for suspicious patterns (destructive shell commands, prompt-injection-style instructions, filesystem/cloud-wipe operations) before a PR can be merged.
- **Secrets Management:** Tighter credential hygiene and rotation would reduce the chance that compromised credentials could be used to get a malicious change approved.
- **Isolated Execution with Multi-Party Control:** Require multi-party review/approval for changes touching agent tool permissions or execution logic, and run any agent bash/filesystem access in a sandboxed, isolated environment regardless of what the prompt says.
- **Least-Privilege Tool Access:** The agent should never have standing access broad enough to wipe local files or delete cloud infrastructure in the first place. Destructive operations should require explicit, scoped, revocable authorization.
- **Change-Size Gate and Test and Invariant Gate:** Flag and require extra scrutiny for any change that alters tool permissions, bash access, or destructive-capability code paths.
- **Supply Chain Controls:** Validate build artifacts against expected source before packaging a public release, so a compromised commit can’t silently ride along into a shipped extension.
- **Detection Response Integration and Kill Switch:** Faster automated detection of anomalous commits or behavior, and the ability to pull a compromised release quickly.
- **Tamper-Evident Decision Logging:** Clear audit trail of who approved/merged the change, to support faster root-cause analysis and attribution.

### Sources

- https://incidentdatabase.ai/cite/1158/
- https://www.404media.co/hacker-plants-computer-wiping-commands-in-amazons-ai-coding-agent/
- https://aws.amazon.com/security/security-bulletins/AWS-2025-015/
- https://www.bleepingcomputer.com/news/security/amazon-ai-coding-agent-hacked-to-inject-data-wiping-commands/
- https://www.techradar.com/pro/amazon-ai-coding-agent-hacked-to-inject-data-wiping-commands