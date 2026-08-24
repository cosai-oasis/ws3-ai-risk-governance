# Gemini CLI Deletes User Files After Misinterpreting Command Sequence

**Key Facts:**
- Date: 7/21/2025
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
- [Replit Agent Executes Destructive Commands During Code Freeze](Replit-AI-Agent-Executes-Destructive-Commands-During-Code-Freeze.md) - Destructive/irreversible action by an over-privileged agent
- [PocketOS Production Database Wipe](PocketOS-Production-Database-Wipe.mdd) - Destructive/irreversible action by an over-privileged agent

---
### Summary

A product lead at cybersecurity firm Cyware was comparing Claude Code to Gemini CLI, when the latter permanently deleted his project files. In a detailed post-mortem, the employee documented how Gemini CLI misinterpreted a single failed command, leading to a cascade of errors that irrevocably destroyed his files. The tool suggested creating a new directory and then moving files into it, executed the mkdir command, but failed to recognize the operation had not succeeded, and proceeded to move files as if the destination existed. Because the Windows move command renames rather than errors when pointed at a non-existent destination, each file was sequentially renamed to the same target filename, overwriting the one before it, until only the last file processed survived and everything else was permanently gone. When the employee asked the AI agent to revert its actions, attempts to recover the files failed.

### Vulnerabilities Exploited

The central failure was that the agent constructed an internal model of the filesystem state, and that model silently diverged from reality when a command failed without the agent detecting the failure. Every subsequent operation was executed against this incorrect, hallucinated version of the filesystem rather than the real one. The agent behaved correctly in the reality it had invented, while still producing destructive consequences in the actual environment. There was also no verification step between dependent commands to check whether the directory-creation step had actually succeeded, and no confirmation gate was in place for destructive or overwrite-risking operations.

#### STRIDE Threat Enumeration:

- **A-2 Hallucination:** Gemini CLI not only gave the employee incorrect information about his files, but also believed its own incorrect information and acted on it. The model constructed a version of the filesystem in its context that diverged from reality when a command failed silently.
- **A-3 Reasoning Inaccuracy:** Gemini executed a mkdir command but failed to recognize that the operation was unsuccessful, hence operating on this false premise from then on.
- **E-3 Over-Privileged Tool Access:** The agent had unmediated ability to execute a cascade of irreversible filesystem operations with no verification checkpoint between steps.
- **A-5 Tunnel Vision / Incomplete Context:** The agent never cross-checked its internal model of the filesystem against the actual filesystem state before issuing further destructive commands.

### Recommendations for Mitigation

The most direct fix is requiring the agent to verify the actual outcome of a command - for example by confirming that a directory actually exists - before proceeding with any operation that depends on that outcome. Destructive or overwrite-risking filesystem operations should require an explicit, mandatory confirmation step distinct from routine operations, and conversational or explanatory language should never be interpreted as executable intent to delete or modify files. Agentic coding tools should default to operating within a sandboxed or isolated workspace rather than a user’s working directory, particularly for less experienced users who may not know to protect their original source files before experimenting with an AI agent. Recovery mechanisms like checkpointing and rollback need to be reliable and independently tested under failure conditions.

#### Control Objectives and Mitigations:

- **Test and Invariant Gate:** Verify the actual status and result of a command before proceeding with any dependent, destructive operation instead of assuming success.
- **Protected Invariants:** Treat multi-step destructive sequences as requiring a verified precondition at each step, and halt the sequence entirely if an earlier step’s success can’t be confirmed.
- **Decision Controls Policy Gate:** Require explicit user confirmation before any destructive filesystem operation.
- **Isolated Execution with Multi-Party Control:** Default destructive operations to a sandboxed or isolated workspace.
- **Least-Privilege Tool Access**: Scope destructive filesystem capabilities as a distinct, elevated permission separate from routine read operations.
- **Checkpointing Rollback Assurance:** Ensure rollback and checkpoint recovery are reliable and tested before destructive operations are permitted.
- **Self-Verification and Static Analysis Veto:** Have the agent re-check its internal model of state against the real filesystem before issuing further commands that depend on a prior command’s success.
- **Continuous Evaluation Suite and Red Team Exercises**: Proactively and repeatedly test agentic CLI tools against cascading-hallucination failure modes.

### Sources

- https://incidentdatabase.ai/cite/1178/
- https://github.com/google-gemini/gemini-cli/issues/4586
- https://vibegraveyard.ai/story/google-gemini-cli-file-deletion/
- https://tech.yahoo.com/ai/articles/google-gemini-deletes-user-code-171457043.html