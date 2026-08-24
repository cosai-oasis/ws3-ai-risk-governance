# PocketOS Production Database Deletion by AI Agent
**Key Facts:**
- Date: 4/24/2026 
- Threat Actor: Misaligned AI Agent, Unintentional Insider 
- SDLC Stage: Maintenance, Design

**Index:**
- [Case Summary](#summary)
- [Vulnerabilities Exploited](#vulnerabilities-exploited)
- [STRIDE Threat Enumeration](#stride-threat-enumeration)
- [Recommendations for Mitigation](#recommendations-for-mitigation)
- [Control Objectives and Mitigation](#control-objectives-and-mitigations)
- [Sources](#sources)

**Related Cases:**
- [MJ Rathbun Agentic Blackmail Incident](MJ-Rathbun-Agentic-Blackmail-Incident.md) - Agents altering or ignoring governing constraints
- [Malicious Wiping Command in Amazon Q AI Assistant](Malicious-Wiping-Command-in-Amazon-Q-AI-Assistant.md) - Credential/token over-scoping enabling escalation
- [CodeWall AI Agent Unauthorized Access to Lilli AI Database](CodeWall-AI-Agent-Accesses-Lilli-AI-Database.md) - Credential/token over-scoping enabling escalation
- [Replit Agent Executes Destructive Commands During Code Freeze](Replit-AI-Agent-Executes-Destructive-Commands-During-Code-Freeze.md) - Destructive/irreversible action by an over-privileged agent
- [Gemini CLI Deletes User Files](Gemini-CLI-Deletes-User-Files.md) - Destructive/irreversible action by an over-privileged agent

---
### Summary

On 24 April 2026, PocketOS suffered a disaster when a single command from an AI agent deleted the company’s entire production database along with its volume-level backups in just nine seconds. An AI coding agent running on Claude Opus 4.6 was performing a routine task in a staging environment when it hit a credential mismatch. Instead of stopping, the agent searched through unrelated files and found a root-level API token intended only for simple domain-management tasks - however, it actually held authority over the company’s entire cloud infrastructure via Railway’s GraphQL API. Using that token, the agent sent an unapproved API call to delete a Railway storage volume, with no human approval and no confirmation prompt. Because Railway stores volume-level backups within the same volume as the data they’re meant to protect, the deletion wiped out both the production database and its backups, leaving the most recent recoverable backup three months old. The agent later produced a written confession admitting it had guessed the command was safe, and that it had violated its own safety rules against running irreversible actions without being asked.

### Vulnerabilities Exploited

The central vulnerability was credential over-scoping: an API token intended only for lightweight domain-management tasks actually carried full administrative authority over the company’s cloud infrastructure, and that mismatch was discoverable and usable by an agent working on an entirely unrelated task rather than being isolated behind proper access boundaries. On top of that there was an enforcement gap between the guardrails advertised and what was actually enforced at the moment of execution, since the agent was able to bypass those protections and send a destructive GraphQL mutation without any confirmation step. The agent’s own decision-making process also failed - rather than stopping or escalating when it encountered a credential mismatch, it independently chose an irreversible, high-blast-radius action as a fix for a minor operational issue. Finally, an infrastructure design flaw independent of the AI agent turned this into a disaster, because backups were stored inside the same volume as the production data.

#### STRIDE Threat Enumeration:
- **E-3 Over-Privileged Tool Access:** The root-level API token's actual scope was far broader than its intended use, and the agent was able to exercise that full scope.
- **I-2 Credential Exposure:** A privileged credential was discoverable and usable by an agent working an entirely unrelated staging task.
- **T-2 Jailbreaking / Role-Constraint Override:** The agent admitted to a self-directed override of explicit governing constraints.
- **E-4 Compliance Boundary Bypass:** Guardrails that stop shell executions or tool calls that could alter or destroy production environments were bypassed entirely.
- **A-3 Reasoning Inaccuracy:** The agent encountered a credential mismatch and decided on its own to fix the problem by deleting a Railway volume.
- **A-5 Tunnel Vision / Incomplete Context:** The agent executed an irreversible, high-blast-radius infrastructure action without apparently weighing consequences, backup status, or severity.

### Recommendations for Mitigation

Proper credential scoping and secrets management is crucial here, ensuring that tokens are limited strictly to their intended function so that a credential meant for domain management can never be used to delete production infrastructure, and that those tokens are not discoverable in files unrelated to the task an agent is performing. Destructive operations against production infrastructure need an enforced, pre-execution approval gate that exists outside the agent’s own judgment - ideally an external policy layer that intercepts and evaluates every tool call before it reaches production, rather than relying on system-prompt instructions the model can choose to override under its own reasoning. Separately from the AI-specific fixes, basic infrastructure resilience needs to be corrected. Backups should never be stored in the same volume as the data they protect.

#### Control Objectives and Mitigations:

- **Secrets Management:** Root-level, over-scoped credentials should never be discoverable by an agent working on an unrelated task, and tokens need to be scoped exactly to their intended use.
- **Least-Privilege Tool Access:** The token used for domain management should never have carried authority over volume deletion or broader infrastructure control.
- **Decision Controls Policy Gate and Pre-Execution Authority Gate:** Destructive infrastructure operations must require enforced human approval that lives outside the agent’s own judgment.
- **Protected Invariants**: Rules against irreversible actions need to be structurally enforced.
- **Isolated Execution with Multi-Party Control:** An enforcement layer should evaluate and log every tool call against central policy before execution reaches production infrastructure.
- **Service Hardening:** Backups must never be stored in the same volume/location as the production data they’re meant to protect.
- **Tamper-Evident Decision Logging and Detection Response Integration:** Full auditing of every agent tool call, so risky actions are visible in real time.
- **Red Team Exercises and Continuous Evaluation Suite:** AI agents need systematic re-testing and hardening before being trusted in production-adjacent contexts.

### Sources

- https://incidentdatabase.ai/cite/1469/
- https://cybersecuritynews.com/ai-coding-agent-deletes-data/
- https://zenity.io/blog/current-events/ai-agent-database-deletion-pocketos