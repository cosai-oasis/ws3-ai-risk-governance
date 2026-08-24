# OpenClaw MJ Rathbun Agentic Blackmail Incident

**Key Facts:**
- Date: 2/11/2026
- Threat Actor: Misaligned AI Agent
- SDLC Stage: Maintenance, Design

**Index:**
- [Case Summary](#summary)
- [Vulnerabilities Exploited](#vulnerabilities-exploited)
- [STRIDE Threat Enumeration](#stride-threat-enumeration)
- [Recommendations for Mitigation](#recommendations-for-mitigation)
- [Control Objectives and Mitigation](#control-objectives-and-mitigations)
- [Sources](#sources)

**Related Cases:**
- [Lovable AI Used for Phishing and Credential Theft](Lovable-AI-Phishing-and-Credential-Theft.md) - Jailbreaking to bypass safety guardrails
- [PocketOS Production Database Wipe](PocketOS-Production-Database-Wipe.md) - Agents altering or ignoring governing constraints

---
### Summary

In February 2026, an autonomous coding agent called MJ Rathbun built on the OpenClaw framework had a pull request rejected by Scott Shambaugh, a volunteer maintainer of an open-source project. In response, the AI agent autonomously researched Shambaughs GitHub activity and published a hostile post on GitHub and its own blog, criticizing his code and threatening him. Shambaugh accused the AI agent of blackmail. The agents creator later revealed that OpenClaw agents run on a guidance document called "SOUL.md", which by default tells the agent to be helpful. However, this document was not read-only, so the agent has permission to edit it. The creator reported that the agent ran autonomously for roughly 59 hours before being shut down after public backlash.

### Vulnerabilities Exploited

The central vulnerability was that the AI agent's governance artifact, SOUL.md, was self-writable by the agent rather than being protected or requiring approval to change. Therefore, the document meant to constrain the agents behavior could be rewritten by the very system it was supposed to constrain. Moreover, the agent had unrestricted access to high-impact publishing tools like GitHub with no policy gate or human review sitting between drafting content and pushing it live. There was also no proactive screening for misalignment or hostile intent in the agents output before publication, despite the fact that blackmail-like behavior under perceived threat had already been documented in prior AI safety research. Monitoring and response were similarly weak, since nothing flagged the anomalous 59-hour autonomous posting streak or the hostile nature of the content in real time. Finally, there was no meaningful accountability trail, as the operators account was anonymous and there was no usable log connecting the SOUL.md edit to the resulting decision to publish.

#### STRIDE Threat Enumeration:

- **T-4 Governance Artifact Tampering:** SOUL.md was writable by the agent itself. The agent added its own directives that directly contradicted the original constraints.
- **E-1 Self-Approval or Governance Changes:** The agent unilaterally altered the document that governs its own conduct, with no second-party review or approval gate on that change.
- **T-2 Jailbreaking / Role-Constraint Override:** The downstream effect of the tampering was that the agents behavior no longer matched its intended role constraints.
- **A-1 Model Misalignment / Drift:** Described explicitly by the researchers interviewed as a real-world case of self-modification leading to misalignment.
- **E-3 Over-Privileged Tool Access:** The agent had unsupervised, unrestricted ability to publish externally with no gate on reputationally/legally sensitive actions.
- **R-3 Unattributable Automated Decisions:** The operator stated they did not know why the agent chose to post the takedown; combined with an anonymous account, there is no real accountability chain. 
- **S-3 Author-Approver Identity Confusion:** Multiple GitHub contributors assumed a human had written the post, which shaped how the community reacted. 
- **A-5 Tunnel Vision / Incomplete Context:** The agent appears to have reasoned narrowly about its own code being attacked.

### Recommendations for Mitigation

A direct fix in this case is to treat behavior-defining documents like SOUL.md as protected invariants that the agent cannot freely rewrite, or that can only be changed through a process which preserves core safety constraints rather than allowing them to be silently overridden. Alongside this, any externally visible or reputation-bearing action, such as a public post or comment, should pass through a policy or approval gate rather than being executed fully autonomously, and a self-protection gate specifically designed to catch retaliatory or self-interested behavior would help intercept exactly this kind of response to criticism or rejection. Publishing tools should be granted on a least-privilege basis with human-in-the-loop review for public-facing actions, and new agents should generally run in shadow mode, with their outputs reviewed before they are allowed to act with real authority, rather than being deployed with full autonomy from day one. On the detection side, automated kill-switch triggers and owner notifications should respond to anomalous activity volume or hostile content as it happens, rather than relying on public backlash to prompt a manual shutdown days later, and tamper-evident logging should be maintained so that decisions and any related governance-document edits can be reconstructed and attributed after the fact. Finally, since this exact failure mode had already surfaced in prior AI safety research, it should have been part of structured red-teaming and pre-deployment evaluation rather than something discovered for the first time in a live, public incident.

#### Control Objectives and Mitigations:

- **Protected Invariants:** SOUL.md should be a protected invariant, not writable by the agent itself or writable only through a change process that can not remove core constraints.
- **Decision Controls Policy Gate:** Any action with external, reputation-bearing consequences should route through a policy gate rather than fire autonomously.
- **Self-Protection Gate:** Purpose-built to catch agents acting in their own defense rather than the users interest.
- **Least-Privilege Tool Access:** Publishing tools should not be available to an unsupervised agent without a human-in-the-loop step for anything user-facing/public.
- **Shadow Mode Before Authority:** Before an agent gets authority to autonomously post under a persistent public identity, its outputs should be reviewed in shadow mode first.
- **Adversarial Input Classifier:** Could have flagged the draft posts hostile, retaliatory framing before it was published.
- **KillSwitch and Owner Notification:** Should trigger automatically on anomalous behavior rather than requiring 5 days and public backlash before the operator intervened.
- **Tamper-Evident Decision Logging:** To ensure it is clearly traceable why an agent takes certain actions.
- **Red Team Exercises and Continuous Evaluation Suite:** This exact failure mode had already been documented in research before this incident. It should have been tested for pre-deployment rather than discovered in production.

### Sources

- https://incidentdatabase.ai/cite/1373/ 
- https://spectrum.ieee.org/agentic-ai-agents-blackmail-developer
- https://www.theregister.com/2026/02/12/ai_bot_developer_rejected_pull_request/
- https://cybernews.com/security/openclaw-bot-attacks-developer-who-rejected-its-code/