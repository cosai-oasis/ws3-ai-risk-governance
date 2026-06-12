<img src="https://github.com/cosai-oasis/oasis-open-project/blob/main/artwork/cosai-logo.png" width="150">

# Agentic Coding Threat Modeling

This directory is the working area for the CoSAI Workstream 3 threat modeling effort for AI-assisted and agentic coding systems. It supports the WS3 SIG Security of AI-Assisted Code Development, especially Deliverable 2: Defense-in-Depth Tooling Blueprint.

The goal is to help practitioners identify, prioritize, and mitigate threats across the AI-assisted software development lifecycle without tying the work to a single model, coding assistant, repository platform, CI/CD system, or cloud provider.

## Purpose

This effort will produce practical threat modeling artifacts that security teams, platform teams, and development teams can adapt to their own environments.

Expected outputs include:

* Threat and controls tables organized by software development lifecycle stage
* Control mappings to relevant secure development and AI security frameworks
* Reference trust boundaries and data flow diagrams for common AI-assisted coding workflows
* Threat trees or other visualization formats for communicating risk
* Machine-readable formats, prompts, or skills that help teams adapt the threat model to their own systems
* User stories and stakeholder needs that keep the work grounded in practitioner workflows

## Scope

The system under analysis is the AI-assisted code generation and delivery pipeline, including:

* Planning and requirements
* Prompting and task decomposition
* Code generation
* Human and AI-assisted review
* Merge and build workflows
* Testing, QA, and evaluation
* Deployment
* Post-deploy monitoring, feedback, and resilience controls

Phase 1 focuses on human-directed coding projects, where a human decides what solution or code should be built and uses AI assistance to implement or review it.

Phase 2 may expand into AI-directed coding projects, where agents initiate or adapt work based on external feedback, product telemetry, or monitoring signals.

In scope:

* Threats to generated code, generated output provenance, review quality, pipeline integrity, and deployment safety
* Trust boundaries between developers, AI assistants, generated output, reviewers, CI/CD systems, security tools, deployment systems, and oversight roles
* Security, privacy, resilience, and governance controls that can be applied during the SDLC
* Threats that arise from biased, misaligned, or unreliable model output when those threats affect coding and delivery workflows

Out of scope for this work:

* Full upstream model provenance, model training, or vendor model lifecycle threat modeling
* A single mandated toolchain or one-size-fits-all enterprise architecture
* Replacing Project CodeGuard, secure coding standards, or existing DevSecOps controls

## Primary Audiences

| Stakeholder | What They Need From This Work |
| :-- | :-- |
| Development engineers | Clear guidance for using AI coding assistants without introducing avoidable security, privacy, or quality risks. |
| AppSec engineers | Threats, controls, review patterns, and mappings that can be operationalized in secure development workflows. |
| Platform engineers | Reference integration patterns for CI/CD, policy enforcement, provenance, scanning, and telemetry. |
| CISO and security staff | A defensible risk view, control taxonomy, and prioritization model for AI-assisted coding adoption. |
| GRC and compliance teams | Mappings from technical controls to higher-level governance, audit, and reporting needs. |

## Relationship to Project CodeGuard

Project CodeGuard is an important input for secure coding rules and AI-assisted review patterns, but this threat model is broader. The threat model should identify where CodeGuard rules help, where other controls are needed, and where gaps exist.

Known areas to evaluate include:

* Whether CodeGuard rules should be mapped to threat model controls
* How CodeGuard can support both code generation and code review workflows
* Whether privacy guidance should be added or expanded in CodeGuard-adjacent controls
* Where threat modeling needs to go beyond CodeGuard into SDLC, governance, provenance, monitoring, and human oversight concerns

## Good First Engagement

Good first contributions include:

* Add or refine a stakeholder user story
* Add threats for one SDLC stage
* Propose a reference architecture source or diagram pattern
* Map a threat to an existing CodeGuard rule, OWASP control, or secure development practice
* Identify a privacy, provenance, human oversight, or monitoring gap
* Convert a narrative threat into a structured row for a threat and controls table
* Suggest a visualization format for threat trees or control mapping

When opening an issue or pull request, please include the relevant SDLC stage, stakeholder, affected trust boundary, and any known control mapping.

## Meetings and Working Norms

The threat modeling group works through a mix of synchronous and asynchronous collaboration:

* Weekly sync: Fridays at 8:00 AM Pacific Time on the official CoSAI calendar
* Async collaboration: GitHub issues, GitHub discussions, and CoSAI Slack
* Running notes and draft artifacts may move between shared documents and this repository, but durable working artifacts should be published here when ready for review

Use GitHub issues for open questions, proposed threat rows, artifact requests, and tracking follow-up work.

## News and Updates

Recent updates:

* May 2026: The threat modeling effort was scoped as part of Deliverable 2 for the WS3 SIG Security of AI-Assisted Code Development.
* May 2026: The group agreed to use the WS3 repository for working threat model artifacts.
* May 2026: Initial outputs were identified: threat and controls tables, control mappings, threat tree visualizations, and reusable machine-readable formats.

Future milestone updates should be added here so new participants can quickly understand what changed and where to engage next.

## Contributing

New participants should start with the CoSAI [onboarding guidance](https://github.com/cosai-oasis/oasis-open-project/blob/main/ONBOARDING.md) and the repository [contributing policy](../../CONTRIBUTING.md).

Contributions are welcome as issues, discussions, pull requests, structured threat rows, diagrams, control mappings, and review comments.

## Support

For issues or feature requests, use GitHub issues in this repository.

You can also join the workstream mailing list by sending an empty email to [cosai-risk-governance-ws@lists.oasis-open-projects.org](mailto:cosai-risk-governance-ws+subscribe@lists.oasis-open-projects.org). The mailing list archive is available [here](https://lists.oasis-open-projects.org/g/cosai-risk-governance-ws/topics).

CoSAI Slack is available via the link in the root repository README. Introduce yourself in the `#ws3-risk-governance` channel and mention the agentic coding threat modeling effort.

## Governance and Licenses

CoSAI and this workstream operate under the [Open Project Rules](https://www.oasis-open.org/policies-guidelines/open-projects-process), [CoSAI Governance](https://github.com/cosai-oasis/oasis-open-project/blob/main/GOVERNANCE.md), and [Workstream Governance](https://github.com/cosai-oasis/oasis-open-project/blob/main/TSC-WS-GOVERNANCE.md).

Documentation and data contributions are expected to use CC-BY 4.0. Source code and model contributions are expected to use Apache License v2.0 unless otherwise specified by the repository.
