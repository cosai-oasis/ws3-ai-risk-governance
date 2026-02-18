# WS3 SIG Security of AI-Assisted Code Development

## MISSION STATEMENT

SIG1 is dedicated to hardening the security posture of software as organizations rapidly scale their use of AI coding assistants. Leveraging the donation of Project CodeGuard as a critical accelerant, this SIG will synthesize model-agnostic technical capabilities with broader industry insights to establish a unified standard and reference implementation for securing AI-assisted code.

## CORE CHALLENGE

AI coding assistants accelerate code generation by an order of magnitude, far outstripping traditional human review and security tooling capacities. The core challenge is striking a balance between utility, defined as the aggregate of volume, velocity, and value, and security.

This challenge is compounded by human factors: automation bias (over-trust in AI output) and skill atrophy can mask novel technical threats including prompt injection, memorization leaks, and non-deterministic hallucinations. SIG1 bridges this gap by delivering scalable, adaptable technical controls that satisfy high-level control objectives and external governance frameworks.

## KEY ASSUMPTIONS

* Organizations have existing DevSecOps pipelines into which controls will be integrated.
* AI coding assistants are being used in non-trivial production workflows, not solely for prototyping.
* A security engineering function exists to consume, configure, and operate the framework outputs.
* Governance alignment targets include CoSAI-RM, Microsoft Responsible AI, SLSA, NIST AI RMF, and OWASP ASVS.

## DELIVERABLES

### DELIVERABLE 1: Adaptable Enterprise Control Framework

#### Target Personas: CISO / Security Leadership  |  GRC / Compliance  |  AppSec Engineers

A modular toolkit and reference framework designed for rapid adoption. Maps granular, adaptable technical implementations to broader organizational governance, allowing enterprises to select and configure controls that match their specific risk appetite and utility requirements.

**Standardized Rule Definitions (Priority: Immediate):**  Machine-readable governance formats (e.g., skills.md, repository-hosted configs) ensuring adaptable technical implementations are consistently enforced across diverse tools and agents.

**Flexible Policy Synthesis:**  A mapping framework linking granular technical rules (from CodeGuard) to high-level control objectives (from CoSAI and internal policy), achieving compliance through adaptable implementation rather than rigid mandates.

**Risk-Based Review Protocols (Priority: Medium-Term):**  Volume-based review protocols that dynamically adjust scrutiny. Low-risk code maintains high velocity via automated checks; high-risk code triggers deeper mandatory gates with higher-capability review.

**Bias-Resistant Verification:**  Specialized human-on-the-loop verification workflows that escalate to mandatory in-the-loop intervention upon detecting high-risk actions or unexpected privilege boundary traversals. Designed to counter automation bias and detect non-standard vulnerabilities such as hallucinated configurations that evade traditional static analysis.

### DELIVERABLE 2: Defense-in-Depth Tooling Blueprint

#### Target Personas: Platform Engineers  |  AppSec Engineers  |  Development Teams

A strategic guide for building a defense-in-depth ecosystem that secures the volume of AI-generated code without degrading development velocity.

**Platform-Agnostic Tooling Guidance:**  Selection criteria for tools that enforce security across diverse environments, preventing vendor lock-in and ensuring consistent posture regardless of AI model.

**Security Conformance and Critical Path Gating:**  Tooling configurations to identify high-risk logic (authentication, cryptography, deserialization, boundary-crossing) triggering human-on-the-loop oversight that escalates to mandatory in-the-loop reviews. Automated validation against secure-coding checklists (OWASP ASVS, CERT).

**Policy as Code:**  Mapping checks to ASVS to ensure compliance is measurable and auditable, not subjective.

**Trust and Provenance Stack:**  Recommendations for combining static analysis with secret scanning and digital signing to maintain a verifiable chain of custody for generated code output. Note: covers provenance of generated output, not provenance of models or vendor tooling.

**Pipeline Integration Patterns:**  Patterns for embedding AI generation constraints into existing DevSecOps pipelines via SAST, DAST, and IAST. Guidance on policy-driven scanners (e.g., Semgrep, CodeQL) to enforce fail-the-build gates. Includes telemetry integration guidance for SIEM and monitoring platforms.

**Threat Modeling (STRIDE):**  Threat model identifying specific threats that could bypass controls, with emphasis on where the value dimension of generated code is most at risk.

### DELIVERABLE 3:  AI-Assisted Security Hardening Methodology

#### Target Personas:  AppSec Engineers  |  Platform Engineers  |  Security Leadership  
 
A reference implementation designed to close the gap between AI generation speed and human review capacity. Defines how organizations can deploy AI to review AI-generated code at scale.

**AI-on-AI Review Architecture:**  Reference architecture for AI review agents utilizing a composite of CodeGuard rules and historical vulnerability data to review high-volume output at machine speed. Includes minimum detection benchmarks for known vulnerability classes and a decision matrix defining when AI review is sufficient versus when human review is mandatory.

**Optimized Human Oversight Model:**  A human-on-the-loop workflow where AI handles bulk syntax and security hygiene checks, elevating only complex logic or high-risk architectural decisions to human engineers.

**Self-Healing Code Patterns:**  Design patterns and policies for validation-failure-triggered automated refinement loops that correct security issues before they reach human review. Note: defines the pattern and policy for these loops, not the agent runtime that executes them.

