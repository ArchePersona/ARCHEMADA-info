# ARCHEMADA

**Tell it what you want built. Work through the plan. Let the system carry the build forward.**

ARCHEMADA is a software-engineering application within the ARCHETRON ecosystem built by **VOLSHi**. It turns a software request into a controlled, inspectable build process.

It does not treat autonomous software construction as a single prompt. ARCHEMADA separates planning, approval, execution, verification, persistence, and review so the user can see what the system intends to do, authorize it, and inspect what actually happened.

> This repository is the public information surface for ARCHEMADA. The private implementation repository is the source of truth for current behavior. This repository mirrors externally relevant product information without publishing private source code or proprietary implementation details.

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

BuildPrint state is persisted and account-owned. The current lifecycle includes:

```text
DRAFT -> APPROVED
  |         |
  +-------> CANCELLED
```

Approval is ownership-checked and idempotent. Approved content is treated as immutable; revision creates a new DRAFT lineage rather than rewriting the historical artifact.

A BuildPrint also carries planning provider/model provenance and the authorized repository/workspace destination used by execution.

## Durable Workspace Authority

ARCHEMADA does not allow a production build to rely on an unnamed or ephemeral-only destination.

The temporary filesystem used during execution is working space. The durable workspace remains the project authority.

## Provider Roles

ARCHEMADA resolves model work into three explicit roles:

- **PLAN** — planning/interview reasoning;
- **BUILD** — software construction;
- **VERIFY** — result evaluation.

Providers are implementation choices behind application-owned boundaries rather than the product itself.

## ARCHESTRATOR

ARCHEMADA is the application. **ARCHESTRATOR** is the reusable engineering lifecycle beneath it.

ARCHEMADA owns the user-facing planning, BuildPrint lifecycle, identity/workspace authority, execution initiation, provider configuration, admission boundary, and presentation of state.

ARCHESTRATOR carries the approved engineering job through the bounded execution lifecycle inside the materialized workspace.

## Deterministic Boundaries

Critical authority decisions remain outside model discretion. Implementation-owned boundaries include:

- BuildPrint lifecycle and content hashing;
- account ownership checks;
- durable workspace readiness;
- repository/workspace normalization;
- provider role resolution;
- materializer dispatch;
- pre-admission provider capability checks;
- execution admission;
- deterministic verification checks; and
- durable writeback requirements.

The model reasons inside the engineering job. It does not define the authority around the job.

## Documentation

- [Product Overview](docs/PRODUCT.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Build Lifecycle](docs/WORKFLOW.md)
- [Security](SECURITY.md)
- [Support](SUPPORT.md)
- [License](LICENSE)

## Development Status

ARCHEMADA is active, early-stage software. The private implementation repository is authoritative when public documentation and current code diverge.

## ARCHETRON

ARCHETRON is the VOLSHi technology ecosystem. ARCHEMADA's responsibility within that ecosystem is the software-engineering application experience and the controlled path from user intent to an inspectable software result.

## Repository Scope

`ARCHEMADA-info` is intended for public product documentation, technical orientation, and other information that can be shared without exposing the private implementation.

Publication of this repository does not grant access to ARCHEMADA source code, private systems, non-public interfaces, or VOLSHi intellectual property.

---

Copyright © 2026 VOLSHi. All rights reserved.
