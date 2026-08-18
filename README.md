# ARCHEMADA

**Tell it what you want built. Work through the plan. Let the system carry the build forward.**

ARCHEMADA is an ARCHETRON software-engineering application for turning a software request into a controlled, inspectable build process.

It does not treat autonomous software construction as a single prompt. ARCHEMADA separates planning, approval, execution, verification, persistence, and review so the user can see what the system intends to do, authorize it, and inspect what actually happened.

> This repository is the public information surface for ARCHEMADA. It does not contain the private application source code or proprietary implementation details.

## The Problem

AI models can generate code. That is not the same thing as reliably delivering software.

A real build has to answer questions that a chat transcript alone does not reliably answer:

- What exactly is being built?
- Which decisions were made before execution?
- What did the user approve?
- Where does the project live?
- Which model/provider performed each stage?
- What actually changed?
- What passed verification?
- Did the result survive the temporary execution environment?

ARCHEMADA is built around those engineering boundaries.

## What ARCHEMADA Does

A typical ARCHEMADA workflow is:

1. **Describe the software** in ordinary language.
2. **Resolve consequential planning decisions** through a guided interview.
3. **Create a BuildPrint** — the durable engineering plan for the job.
4. **Review and approve** that BuildPrint before paid execution begins.
5. **Materialize an authorized workspace** into a bounded execution environment.
6. **Execute the build through ARCHESTRATOR.**
7. **Verify the result** using deterministic checks and model-backed evaluation where appropriate.
8. **Write the result back** to the authorized durable workspace.
9. **Preserve execution state and provenance** so the result can be inspected after the run.

The objective is straightforward: make autonomous software engineering easier to supervise, explain, and trust.

## BuildPrint

The BuildPrint is the central user-visible contract between planning and execution.

It records the project objective, assumptions, capabilities, data requirements, integrations, constraints, deliverables, and the authorized project destination. A BuildPrint is created as a draft, explicitly approved, and then treated as the engineering authority for the build.

Approval is a real lifecycle boundary. ARCHEMADA does not silently reinterpret the approved destination or substitute a different workspace during execution.

## Durable Workspaces

ARCHEMADA separates temporary execution space from durable project authority.

Current workspace paths include:

- **Google Drive** — an explicitly selected/authorized Drive folder can be materialized for execution and synchronized back after the build.
- **GitHub** — an authorized repository can serve as the durable source and destination for a build.

The build workspace inside the execution service is ephemeral. The durable source remains the authority.

## ARCHESTRATOR

ARCHEMADA is the application. **ARCHESTRATOR** is the reusable engineering lifecycle beneath it.

ARCHEMADA owns the user-facing planning, BuildPrint lifecycle, account/workspace boundary, execution controls, provider selection, billing/admission boundary, and presentation of results.

ARCHESTRATOR owns the bounded engineering execution lifecycle used to carry approved software work forward.

Keeping those responsibilities separate lets the application experience and the underlying engineering engine evolve independently.

## Current Google Cloud Profile

The current ARCHEMADA application uses Google infrastructure including:

- **Gemini 3.7 Flash** through **Vertex AI** for model-backed planning/build/verification paths;
- **Cloud Run** for the application service/execution boundary;
- **Firestore** for durable account, provider, BuildPrint, and execution state;
- **Firebase Authentication** for Google account identity;
- **Firebase Hosting** for the browser application; and
- **Google Drive API** for authorized durable workspace materialization and writeback.

Provider/model selection remains explicit. BUILD and VERIFY can inherit the approved PLAN provider/model when no independent override is selected.

## Deterministic Boundaries

ARCHEMADA deliberately keeps several critical decisions outside model discretion.

Examples include:

- BuildPrint lifecycle state;
- workspace readiness and destination identity;
- repository/workspace normalization;
- execution admission and billing boundaries;
- materializer dispatch;
- provider inheritance;
- deterministic verification checks; and
- writeback completion requirements.

The model can reason inside the job. It does not get to redefine the job's authority boundaries.

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

ARCHEMADA is active, early-stage software. Public documentation describes current product behavior and externally relevant architecture; it is not a promise that every interface or deployment detail will remain unchanged.

## ARCHETRON

ARCHEMADA is an ARCHETRON technology. Within the broader system, its responsibility is deliberately narrow: **the software-engineering application experience and the controlled path from user intent to an inspectable software result.**

## Repository Scope

`ARCHEMADA-info` is intended for public product documentation, technical orientation, evaluation material, and other information that can be shared without exposing the private ARCHEMADA implementation.

Publication of this repository does not grant access to ARCHEMADA source code, private systems, non-public interfaces, or ARCHETRON intellectual property.

---

Copyright © 2026 ARCHETRON. All rights reserved.
