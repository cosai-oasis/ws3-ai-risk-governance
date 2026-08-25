
### Threat Actors

- Unintentional Insider (Human)
- Misaligned AI Agent (Agent)
- Malicious Insider (Human)
- Compromised AI Agent (Agent)
- External Adversary

### STRIDE Threat Enumeration

- Spoofing
	- S-1 Approval Credential Theft or Misuse
	- S-2 Spoofed Repository Events
	- S-3 Author-Approver Identity Confusion
- Tampering
	- T-1 Prompt Injection
	- T-2 Jailbreaking / Role-Constraint Override
	- T-3 Compromised Internal Data Source
	- T-4 Governance Artifact Tampering
	- T-5 Runtime Service Compromise
	- T-6 Reviewer Infrastructure Supply Chain
- Repudiation
	- R-1 Audit Trail Gaps
	- R-2 Mutable or Self-Writable Audit Logs
	- R-3 Unattributable Automated Decisions
- Information Disclosure
	- I-1 Data Exfiltration via Tool Calls
	- I-2 Credential Exposure
	- I-3 Over-Broad Context Exposure
- Denial of Service
	- D-1 Context Window Exhaustion
	- D-2 Change-Size Exploitation
	- D-3 Review Pipeline Flooding
- Elevation of Privileges
	- E-1 Self-Approval or Governance Changes
	- E-2 Over-Privileged Reviewer Identity
	- E-3 Over-Privileged Tool Access
	- E-4 Compliance Boundary Bypass
	- E-5 Reward Hacking by an AI Author
- AI-Intrinsic Risks (Beyond STRIDE)
	- A-1 Model Misalignment / Drift
	- A-2 Hallucination
	- A-3 Reasoning Inaccuracy
	- A-4 Knowledge Blind Spots
	- A-5 Tunnel Vision / Incomplete Context
	- A-6 Generic vs Specific Gap
	- A-7 Missing or Misunderstood Context

### Control Objectives and Mitigations

- Input and Decision Integrity Controls
	- Primary Review Classifier (bugs and correctness)
	- Adversarial Input Classifier
	- Risk-Tier Classifier
	- Compliance Scope Classifier
	- Code Quality Classifier
	- Intent Classifier
	- Project-Context Evaluation
	- Static Analysis Veto
	- Test and Invariant Gate
- Scope and Eligibility Controls
	- Decision Controls Policy Gate
	- Self-Protection Gate
	- Change-Size Gate
	- Protected Invariants
	- Owner Opt-Out
- Tool and Integration Controls
	- Least-Privilege Tool Access
	- Tool Output Sandboxing
	- Outbound Data Controls
	- Internal Source Integrity
- Infrastructure and Identity Controls
	- Least Privilege Reviewer Identity
	- Secrets Management
	- Isolated Execution with Multi-Party Control
	- Service Hardening
	- Supply Chain Controls
- Audit, Detection, and Response Controls
	- Tamper-Evident Decision Logging
	- Detection & Response Integration
	- Owner Notification
	- Kill Switch
- Assurance and Continuous Validation
	- Shadow Mode Before Authority
	- Risk-Weighted Human Sampling
	- Adversarial Change Pipeline
	- Red Team Exercises
	- Continuous Evaluation Suite