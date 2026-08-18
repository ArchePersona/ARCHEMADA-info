# ARCHEMADA

**Tell it what you want built. Work through the plan. Let the system carry the build forward.**

ARCHEMADA is an ARCHETRON software-engineering application for turning a software request into a controlled, inspectable build process.

It does not treat autonomous software construction as a single prompt. ARCHEMADA separates planning, approval, execution, verification, persistence, and review so the user can see what the system intends to do, authorize it, and inspect what actually happened.

> This repository is the public information surface for ARCHEMADA. The private `ArchePersona/Archemada` implementation repository is the source of truth for current behavior. This repository mirrors externally relevant product and evaluation information without publishing private source code or proprietary implementation details.

## The Problem

AI models can generate code. That is not the same thing as reliably delivering software.

A real build has to answer questions a chat transcript alone does not reliably answer:

- What exactly is being built?
- Which decisions were made before execution?
- What did the user approve?
- Where does the project live?
- Which provider/model performed each stage?
- What actually changed?
- What passed verification?
- Did the result survive the temporary execution environment?

ARCHEMADA is built around those engineering boundaries.

## Current Product Flow

The current implementation follows this path:

1. **Describe the software** in ordinary language.
2. **Resolve consequential planning decisions** through a structured interview.
3. **Create a BuildPrint DRAFT** as durable engineering state.
4. **Review and explicitly approve** the BuildPrint.
5. **Require an authorized durable workspace** before execution.
6. **Revalidate workspace and provider capability before billing admission.**
7. **Materialize the durable workspace** into an ephemeral execution directory.
8. **Execute the approved job through ARCHESTRATOR.**
9. **Verify the result.**
10. **Write the result back** to the authorized durable workspace.
11. **Preserve execution state and provenance** for inspection.

## BuildPrint

The BuildPrint is the durable contract between planning and execution.

BuildPrint state is persisted in Firestore and is account-owned. The current lifecycle includes:

```text
DRAFT -> APPROVED
  |         |
  +-------> CANCELLED
```

Approval is ownership-checked and idempotent. Approved content is treated as immutable; revision creates a new DRAFT lineage rather than rewriting the historical artifact.

A BuildPrint also carries planning provider/model provenance and the authorized repository/workspace destination used by execution.

## Durable Workspace Authority

ARCHEMADA does not allow a production build to rely on an unnamed or ephemeral-only destination.

Current durable workspace types are:

- **Google Drive** — a user-selected Drive resource that the backend has verified and recorded as ready.
- **GitHub** — a canonical repository destination for which the server has write authority.

There is no production default workspace and no silent ephemeral fallback.

The temporary filesystem used during execution is working space. The durable workspace remains the project authority.

## Provider Roles

ARCHEMADA resolves model work into three explicit roles:

- **PLAN** — planning/interview reasoning;
- **BUILD** — software construction;
- **VERIFY** — result evaluation.

The current default provider is **Google Vertex AI**. The native Google path uses **Gemini 3.7 Flash** through the **Google GenAI SDK** with Vertex AI enabled.

Production Vertex authentication uses Application Default Credentials from the runtime identity rather than a browser API key. BUILD and VERIFY may inherit the PLAN provider/model when no independent override is configured.

## ARCHESTRATOR

ARCHEMADA is the application. **ARCHESTRATOR** is the reusable engineering lifecycle beneath it.

ARCHEMADA owns the user-facing planning, BuildPrint lifecycle, identity/workspace authority, execution initiation, provider configuration, billing/admission boundary, and presentation of state.

ARCHESTRATOR carries the approved engineering job through the bounded execution lifecycle inside the materialized workspace.

## Current Google Stack

The private implementation currently uses:

- **Gemini 3.7 Flash** through **Vertex AI**;
- **Google GenAI SDK** (`google-genai>=2.0.0`);
- **Cloud Run** for the backend/execution service;
- **Firestore** for durable application state;
- **Firebase Authentication** for account identity;
- **Firebase Hosting** for the browser application; and
- **Google Drive API** for durable Drive workspace access and writeback.

## Deterministic Boundaries

Critical authority decisions remain outside model discretion. Current implementation-owned boundaries include:

- BuildPrint lifecycle and content hashing;
- account ownership checks;
- durable workspace readiness;
- repository/workspace normalization;
- provider role inheritance;
- materializer dispatch;
- pre-admission provider capability checks;
- billing admission;
- deterministic verification checks; and
- durable writeback requirements.

The model reasons inside the engineering job. It does not define the authority around the job.

## Documentation

### Product documentation

- [Product Overview](docs/PRODUCT.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Build Lifecycle](docs/WORKFLOW.md)
- [Security](SECURITY.md)
- [Support](SUPPORT.md)
- [License](LICENSE)

### All Things Agentic evaluation material

- [Evaluation Index](evaluation/all-things-agentic/README.md)
- [Hackathon Architecture](evaluation/all-things-agentic/ARCHITECTURE.md)
- [Deterministic Engine & Control Boundaries](evaluation/all-things-agentic/DETERMINISTIC-ENGINE.md)
- [Google Technology Stack](evaluation/all-things-agentic/GOOGLE-STACK.md)
- [Project Provenance / New-Project Disclosure](evaluation/all-things-agentic/PROVENANCE.md)
- [Demo Evidence Plan](evaluation/all-things-agentic/DEMO.md)

## Development Status

ARCHEMADA is active, early-stage software. The private implementation repository is authoritative when public documentation and current code diverge.

## ARCHETRON

ARCHEMADA is an ARCHETRON technology. Its responsibility is the software-engineering application experience and the controlled path from user intent to an inspectable software result.

## Repository Scope

`ARCHEMADA-info` is intended for public product documentation, technical orientation, evaluation material, and other information that can be shared without exposing the private implementation.

Publication of this repository does not grant access to ARCHEMADA source code, private systems, non-public interfaces, or ARCHETRON intellectual property.

---

Copyright © 2026 ARCHETRON. All rights reserved.
