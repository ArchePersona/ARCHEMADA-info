# ARCHEMADA Product Overview

## Purpose

ARCHEMADA provides a user-facing environment for controlled autonomous software engineering.

Its purpose is not merely to generate code. It carries a software request through planning, approval, execution, verification, and durable persistence so the user can inspect both the intended work and the resulting work.

The private `ArchePersona/Archemada` implementation repository is the source of truth for current behavior. This public repository mirrors externally relevant behavior without exposing private source code.

## Product Model

ARCHEMADA treats each request as an engineering job with explicit state and explicit authority boundaries.

The user begins with intent rather than implementation detail. The planning system works through unresolved engineering targets, produces a BuildPrint, and waits for explicit approval before execution.

The conversation helps define the job. It is not the durable job record.

## BuildPrint

BuildPrint is persisted as account-owned Firestore state.

A BuildPrint contains the engineering content required for the build together with repository/workspace destination and planning provenance. Content is hashed deterministically.

Current lifecycle behavior includes:

```text
DRAFT -> APPROVED
  |         |
  +-------> CANCELLED
```

Approval is ownership-checked and idempotent. An approved artifact is not rewritten in place. Revision creates a new DRAFT lineage.

## Planning

Planning is structured rather than purely conversational.

Already-established application state can resolve planning targets before the model runs. This is important for facts such as an already-selected durable workspace: the model should not be allowed to re-decide an authoritative destination.

The planning layer also resolves simple application-owned semantics deterministically where appropriate, including numbered option replies when the active question defines those options.

## Durable Workspaces

Production execution requires an authorized durable workspace.

The current implementation supports:

- **Google Drive** — the account's selected Drive resource must match the BuildPrint destination and must have been backend-verified as ready.
- **GitHub** — the destination must be canonical and the server must have write authority.

There is no production default workspace and no silent ephemeral fallback.

The local filesystem used during execution is temporary working space, not project authority.

## Provider Roles

ARCHEMADA resolves model work into explicit roles:

- **PLAN**
- **BUILD**
- **VERIFY**

The current default provider is `vertex_ai` using **Gemini 3.7 Flash**.

The native Google provider is implemented with the **Google GenAI SDK** in Vertex mode. Production authentication uses runtime Application Default Credentials rather than a browser API key.

BUILD and VERIFY can use explicit overrides or inherit the PLAN provider/model through deterministic application configuration.

## Pre-Admission Controls

Before paid execution begins, the implementation establishes several preconditions:

1. the durable workspace is ready;
2. Drive-backed work is re-probed using the live interactive Drive authorization;
3. the effective BUILD provider/model is resolved;
4. Vertex provider capability is initialized when Vertex is selected;
5. only then may the execution cross the billing/admission boundary.

The design intent is that configuration failures happen before build credit is consumed.

## ARCHESTRATOR

ARCHEMADA is the application. ARCHESTRATOR is the reusable engineering engine beneath it.

ARCHEMADA owns:

- browser/user interaction;
- identity;
- planning interaction;
- BuildPrint lifecycle;
- workspace authority;
- provider/model configuration;
- execution initiation;
- billing/admission boundaries; and
- user-visible execution state.

ARCHESTRATOR owns the bounded engineering lifecycle that carries approved work forward inside the prepared workspace.

## Verification and Persistence

Verification is a distinct phase of the job rather than a conversational afterthought.

The application can combine deterministic verification with model-backed verification. Concrete deterministic failures are not meant to be silently converted into success by model interpretation.

For remote workspaces, successful persistence requires writeback to the authorized durable source. Producing files only in a temporary execution directory is not sufficient durable completion.

## Current Google Infrastructure

The current private implementation uses:

- Gemini 3.7 Flash;
- Vertex AI;
- Google GenAI SDK (`google-genai>=2.0.0`);
- Cloud Run;
- Firestore;
- Firebase Authentication;
- Firebase Hosting; and
- Google Drive APIs.

## Development Status

ARCHEMADA is early-stage software under active development. Interfaces and deployment characteristics may change as the system is hardened.

When this public documentation and the private implementation diverge, the private implementation is authoritative.
