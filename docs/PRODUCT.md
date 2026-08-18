# ARCHEMADA Product Overview

## Purpose

ARCHEMADA provides a user-facing environment for controlled autonomous software engineering.

Its purpose is not merely to generate code. It carries a software request through planning, approval, execution, verification, and durable persistence so the user can inspect both the intended work and the resulting work.

## The Problem

Code generation is only one part of software engineering.

A real engineering job also requires the system to determine what the request means, identify dependencies, recognize decisions that require clarification, establish where the software lives, preserve the approved plan, perform the work, verify results, persist output, and report what happened.

When those responsibilities disappear inside an opaque model interaction, the user has little basis for understanding whether the result stayed within the intended scope.

ARCHEMADA is built around that gap.

## Product Model

ARCHEMADA treats each request as an engineering job with explicit state and explicit authority boundaries.

The user begins with intent rather than implementation detail. The system develops a plan through a guided interview, produces a BuildPrint, and waits for approval before execution. After approval, the selected durable workspace is materialized into a temporary execution environment, the build is carried forward through ARCHESTRATOR, the result is verified, and successful output is synchronized back to the durable workspace.

The conversation helps define the job. It is not the durable job record.

## BuildPrint

The BuildPrint is the durable plan between planning and execution.

It can capture:

- project objective;
- assumptions;
- core capabilities;
- user experience requirements;
- data requirements;
- integrations;
- safety and engineering constraints;
- deliverables;
- open questions; and
- the authorized repository/workspace destination.

A BuildPrint starts as a draft. Approval changes its lifecycle state without silently changing the approved content or destination.

## Human Involvement

Different jobs and users require different amounts of interaction during planning.

ARCHEMADA separates planning involvement from execution authority.

Planning modes currently include:

- **Cautious** — asks for more clarification and makes fewer assumptions.
- **Moderate** — asks when a decision materially changes the build and makes safe assumptions where appropriate.
- **Trust Me, Bro** — moves quickly and stops primarily for genuine blockers or consequential decisions.

Greater planning autonomy does not bypass BuildPrint approval, workspace readiness, execution admission, verification, or persistence requirements.

## Durable Workspaces

ARCHEMADA does not treat the temporary execution filesystem as the project authority.

A project is associated with an authorized durable workspace. Current implementations include Google Drive folders and GitHub repositories.

For Google Drive, an explicitly authorized folder can be materialized into an ephemeral execution workspace and synchronized back after successful execution. Workspace readiness is determined by backend authority rather than by browser presentation alone.

## Execution

ARCHEMADA hands an approved engineering job to ARCHESTRATOR.

ARCHESTRATOR is the reusable engineering lifecycle beneath the application. The separation keeps product interaction, authentication, billing/admission, provider configuration, and workspace authority out of the reusable execution engine.

## Verification

Verification is treated as a distinct part of the job rather than a conversational afterthought.

ARCHEMADA can combine deterministic checks with model-backed verification. Deterministic checks remain application-controlled; model-backed verification can reason about whether the result satisfies the approved BuildPrint.

## Persistence

A build is not considered successfully durable merely because files were produced in a temporary execution environment.

For remote workspaces, successful persistence requires writeback to the authorized source. The system records resulting provenance/version identity where available and does not intentionally treat ephemeral-only output as the final project authority.

## Google Cloud

Current ARCHEMADA deployment uses Google infrastructure including Vertex AI / Gemini, Cloud Run, Firestore, Firebase Authentication, Firebase Hosting, and Google Drive APIs.

The application can explicitly resolve model/provider roles for planning, build, and verification. BUILD and VERIFY may inherit the PLAN provider/model when no override is selected.

## Relationship to ARCHESTRATOR

ARCHEMADA is the application. ARCHESTRATOR is the reusable engineering engine underneath it.

ARCHEMADA owns:

- user interaction;
- planning conversation;
- BuildPrint lifecycle;
- identity and workspace authority;
- execution initiation;
- provider/model configuration;
- billing/admission boundaries; and
- user-visible execution state.

ARCHESTRATOR owns the bounded engineering lifecycle that carries approved work forward.

## Development Status

ARCHEMADA is early-stage software under active development. Interfaces and deployment characteristics may change as the system is hardened.

This repository describes the product at a public level and intentionally omits private implementation material.
