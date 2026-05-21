# RepoWarden
## The Living Specification Engine

*"Your code remembers what it does. RepoWarden remembers why."*

---

## The Problem Nobody Has Solved In 70 Years

Software is the only engineering discipline where the blueprint and the artifact are the same object.

When a bridge engineer leaves, the blueprint stays. When a software engineer leaves, the blueprint - the intent, the reasoning, the constraints - leaves with them.

This is why software rots. Why incidents repeat. Why new engineers take 6 months to be productive. Why the same bugs get reintroduced after being fixed. Why technical debt compounds forever.

**RepoWarden solves this.**

---

## What RepoWarden Does

RepoWarden maintains a **Living Specification** - a parallel intelligence layer alongside your codebase that captures not just what your code does, but why it was built that way, what contracts it honors, what decisions shaped it, and what dangers lurk within it.

This specification updates itself on every merge. It validates every MR before it ships. It answers questions on demand. It onboards new engineers in days not months. It catches security violations before they reach production.

It is not documentation. Documentation goes stale the moment it's written.

**This is a living organism that grows smarter with every commit.**

---

## Proven In Production

RepoWarden has been demonstrated on a real payment processing module with fraud detection, webhook security, and compliance requirements. Here is what it produced automatically:

### The Living Specification extracted from a single MR

- **8 architectural decisions** with full reasoning - including why the 30% fraud threshold was chosen after 6-month chargeback analysis showing 87% correlation with fraud
- **9 critical dangers** - including SEC-2024-441, a real timing attack incident exploited in staging
- **Complete API contracts** for every function - inputs, outputs, guarantees, and explicit prohibitions
- **Evolution timeline** - a narrative of what the codebase is becoming and why

### The Guardian caught a real security violation

A developer changed `hmac.compare_digest` to `==` in the webhook signature verification function. RepoWarden fired immediately:

```
🚨 CRITICAL VIOLATION - Danger Pattern Detected

This change recreates the timing attack vulnerability
documented in SEC-2024-441.

From SPEC/dangers/payment_processor.md:
"Never replace hmac.compare_digest with ==.
An attacker reconstructed valid HMAC signatures
byte-by-byte in ~16,000 requests on staging."

Status: VIOLATION - Do not merge without justification.
```

### The Oracle answered an institutional knowledge question

> "Why does verify_webhook_signature use hmac.compare_digest instead of ==? Is it safe to change?"

```
HIGH CONFIDENCE - Directly documented in Living Specification.

Decision 4 (MR !7, 2026-03-23): hmac.compare_digest
was chosen after SEC-2024-441 demonstrated that ==
comparison allowed timing attacks in ~16,000 requests.

Contract: verify_webhook_signature() will NEVER use
== for signature comparison. This is a binding
security requirement, not a style preference.

Do not change without senior security review and
specification update.
```

### The Documentation Guard wrote production docstrings

Every merged function automatically received complete docstrings - Args, Returns, Raises, Examples, and security warnings - without any human intervention. An MR was opened and merged automatically.

### The Security Guard opened an auto-fix MR

Detected the timing attack vulnerability reintroduced by the `==` change, explained it in plain English with zero jargon, and opened a fix MR automatically.

### The Drift Reporter scored 0/100

After the webhook change was introduced, the Drift Reporter caught it in the weekly scan - scoring the payment processor 0/100 for specification compliance and explaining exactly what drifted and why it mattered.

---

## The Five Layers

### 1. Intent Layer - `SPEC/intent/`
*Why every piece of code was built*

Claude reads every MR description, issue discussion, and review comment and extracts the developer's original intent. Not what the code does. What the developer meant it to do. These are permanently different things and the gap between them is where all bugs live.

### 2. Contract Layer - `SPEC/contracts/`
*What every component promises*

Every function's inputs, outputs, guarantees, and explicit prohibitions - documented automatically and validated on every change. Breaking a contract triggers an immediate Guardian report.

### 3. Decision Layer - `SPEC/decisions/`
*Why every architectural choice was made*

What alternatives were considered. What constraints ruled them out. What assumptions underpin the implementation. What would need to change for a different approach to become correct. This is the layer that dies when engineers leave. RepoWarden captures it automatically.

### 4. Danger Layer - `SPEC/dangers/`
*What the codebase has learned through failure*

Every incident, every near-miss, every documented risk - captured permanently as an active constraint that influences every future decision. When a danger pattern reappears in a new MR, the Guardian catches it before it ships.

### 5. Evolution Layer - `SPEC/evolution/`
*How the codebase has changed and why*

A complete narrative of what the codebase was trying to become at each stage of its life. New engineers read this and understand the architecture in hours instead of months.

---

## The Eight Flows

| Flow | Trigger | What It Does |
|------|---------|--------------|
| Documentation Guard | After MR merge | Analyzes changes, writes docstrings, opens documentation MR |
| Living Spec Builder | After MR merge | Extracts intent, contracts, decisions, dangers into SPEC/ |
| Specification Guardian | @mention pre-merge | Validates MR against Living Specification, catches violations |
| Specification Oracle | @mention | Answers any question about the codebase from the specification |
| Specification Onboarding | @mention | Generates personalized onboarding guide from accumulated knowledge |
| Specification Drift Reporter | Weekly / @mention | Identifies where code has drifted from original specification |
| Security Guard | @mention | Explains vulnerabilities in plain English, opens auto-fix MRs |
| Health Reporter | Weekly / @mention | Repository health score covering docs and security posture |

---

## How To Use RepoWarden

### After merging code

```
@ai-repowarden-documentation-guard-gitlab-ai-hackathon
please analyze this MR and update the documentation

@ai-repowarden-living-specification-builder-gitlab-ai-hackathon
please analyze this MR and build the living specification
```

### Before merging a risky change

```
@ai-repowarden-specification-guardian-gitlab-ai-hackathon
please validate this MR against the living specification
```

### When you have a question about the codebase

```
@ai-repowarden-specification-oracle-gitlab-ai-hackathon
why does [function] work this way? Is it safe to change?
```

### When a new engineer joins

```
@ai-repowarden-specification-onboarding-gitlab-ai-hackathon
please generate an onboarding guide for this repository
```

### Weekly health and drift checks

```
@ai-repowarden-specification-drift-reporter-gitlab-ai-hackathon
please generate this week's drift report

@ai-repowarden-weekly-health-report-gitlab-ai-hackathon
please generate a health report for this repository

@ai-repowarden-security-guard-gitlab-ai-hackathon
please analyze the security posture of this repository
```

---

## Repository Structure

```
agents/
├── doc-analyzer.yml          # Documentation analysis agent
├── doc-writer.yml            # Documentation writing agent
├── health-reporter.yml       # Weekly health scoring agent
├── security-guard.yml        # Security vulnerability agent
├── spec-drift-reporter.yml   # Specification drift detection agent
├── spec-intake.yml           # Specification extraction agent
├── spec-onboarding.yml       # Onboarding guide generation agent
├── spec-oracle.yml           # Institutional knowledge Q&A agent
└── spec-validator.yml        # Pre-merge validation agent

flows/
├── flow-docs.yml             # Documentation Guard flow
├── flow-scorecard.yml        # Health Reporter flow
├── flow-security.yml         # Security Guard flow
├── flow-spec-builder.yml     # Living Specification Builder flow
├── flow-spec-drift.yml       # Specification Drift Reporter flow
├── flow-spec-guardian.yml    # Specification Guardian flow
├── flow-spec-onboarding.yml  # Specification Onboarding flow
└── flow-spec-oracle.yml      # Specification Oracle flow

SPEC/
├── INDEX.md                  # Specification coverage metrics
├── contracts/                # What every component promises
├── dangers/                  # What must never happen and why
├── decisions/                # Why architectural choices were made
├── evolution/                # How the codebase has changed over time
│   └── TIMELINE.md
└── intent/                   # Why every component was built

src/
├── calculator.py             # Demo module - simple arithmetic
└── payment_processor.py      # Demo module - payment processing

AGENTS.md                     # RepoWarden configuration
```

---

## Architecture

```
Every MR Merged
      │
      ▼
┌─────────────────┐    ┌──────────────────┐
│ Documentation   │    │ Specification    │
│ Guard           │    │ Builder          │
│                 │    │                  │
│ Writes          │    │ Extracts intent  │
│ docstrings      │    │ Updates SPEC/    │
│ Opens doc MR    │    │ Commits to main  │
└─────────────────┘    └────────┬─────────┘
                                │
                         SPEC/ directory
                         grows over time
                                │
        ┌───────────────────────┼───────────────────────┐
        │                       │                       │
        ▼                       ▼                       ▼
┌──────────────┐      ┌──────────────┐       ┌──────────────┐
│ Specification│      │ Specification│       │ Specification│
│ Guardian     │      │ Oracle       │       │ Drift        │
│              │      │              │       │ Reporter     │
│ Validates    │      │ Answers any  │       │ Weekly decay │
│ pre-merge    │      │ question     │       │ detection    │
│ MRs against  │      │ from the     │       │ scores every │
│ spec         │      │ spec         │       │ component    │
└──────────────┘      └──────────────┘       └──────────────┘

        ┌───────────────────────┬───────────────────────┐
        │                       │                       │
        ▼                       ▼                       ▼
┌──────────────┐      ┌──────────────┐       ┌──────────────┐
│ Security     │      │ Health       │       │ Specification│
│ Guard        │      │ Reporter     │       │ Onboarding   │
│              │      │              │       │              │
│ Explains     │      │ Weekly score │       │ Personalized │
│ vulns in     │      │ docs +       │       │ guide from   │
│ plain English│      │ security     │       │ spec history │
│ opens fix MR │      │              │       │              │
└──────────────┘      └──────────────┘       └──────────────┘
```

---

## What Makes RepoWarden Different

Every other tool in this space operates on **what code does**.

RepoWarden operates on **what code was meant to do**.

| System | What it remembers | How it helps |
|--------|------------------|--------------|
| Git | What changed and who | Audit trail |
| Tests | Specific behaviors | Regression detection |
| SentinelOps | What broke | Prevents future incidents |
| ARGUS | Security findings | Governs merges |
| RepoWarden | Why decisions were made | Preserves institutional knowledge forever |

The distinction is not incremental. It is categorical.

SentinelOps remembers what broke. RepoWarden remembers why it was built the way it was - and catches violations before they break anything.

---

## Setup

### Prerequisites

- GitLab Ultimate or GitLab AI Hackathon sandbox
- GitLab Duo Agent Platform enabled on your project

### Installation

1. Copy `agents/` and `flows/` directories into your project
2. Copy `SPEC/` directory structure into your project
3. Edit `AGENTS.md` to configure team preferences
4. Enable all flows in **Automate → Flows → Managed**
5. Create a tag to publish: `git tag v1.0.0 && git push origin v1.0.0`
6. Trigger the Spec Builder on your first MR:

```
@ai-repowarden-living-specification-builder-gitlab-ai-hackathon
please analyze MR ![number] and build the initial
living specification for this repository
```

---

## Configuration - AGENTS.md

RepoWarden reads `AGENTS.md` at the root of your repository to understand team preferences, coding standards, and project-specific rules. Edit it to customize behavior for your codebase.

---

## Built With

- **GitLab Duo Agent Platform** - flow orchestration and tool execution
- **Anthropic Claude** - intent extraction, decision archaeology, specification validation
- **GitLab REST API** - repository access, MR creation, issue management

---

## Why This Requires Claude

Every capability in RepoWarden is fundamentally impossible without frontier-level language understanding:

- **Intent extraction** requires reading ambiguous human communication and synthesizing meaning from MR descriptions, issue discussions, and review comments
- **Specification validation** requires understanding whether a code change honors the spirit of a decision, not just the letter
- **Institutional Q&A** requires synthesizing history across multiple documents and answering in the voice of the original author
- **Danger pattern detection** requires understanding semantic similarity between a new change and a previously documented failure

You cannot do any of this with rules, regex, or static analysis. This is Claude or nothing.

---

## License

MIT - see LICENSE

---

*Built for the GitLab AI Hackathon 2026*

*"In 1996, Linus Torvalds created Git to track what changed and who changed it. In 2026, RepoWarden captures what was never tracked: why."*
