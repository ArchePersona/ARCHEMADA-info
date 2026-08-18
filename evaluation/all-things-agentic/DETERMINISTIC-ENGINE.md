# Deterministic Engine & Control Boundaries

## Source of Truth

This document is derived from the current private `ArchePersona/Archemada` implementation. The implementation repository is authoritative.

## Why Deterministic Control Exists

ARCHEMADA uses generative models for interpretation, planning, software construction, and higher-level evaluation.

It does not allow the model to own every consequential system decision.

The deterministic layer controls authority, identity, lifecycle, provider resolution, workspace readiness, billing admission, and durable persistence.

## Model Reasoning vs. System Authority

```text
MODEL:
What does the user want?
What implementation should satisfy the BuildPrint?
What changes should be made?
How well does the result satisfy higher-level requirements?

SYSTEM:
Who owns this BuildPrint?
What is its lifecycle state?
What content hash identifies it?
Which durable workspace is authorized?
Is that workspace READY?
Which provider/model is effective for PLAN / BUILD / VERIFY?
May execution cross the billing boundary?
Did concrete verification pass?
Did durable writeback succeed?
```

The model reasons inside the job. The application controls the authority around the job.

## 1. BuildPrint Identity and Hashing

BuildPrint content is serialized canonically and hashed with SHA-256.

The record is account-owned in Firestore and includes lifecycle state, planning provider/model provenance, timestamps, and execution linkage.

The browser renders BuildPrint state; Firestore is authoritative.

## 2. BuildPrint Lifecycle

Current lifecycle behavior includes:

```text
DRAFT -> APPROVED
  |         |
  +-------> CANCELLED
```

Approval is ownership-checked and idempotent.

Approved artifacts are not rewritten in place. Revision creates a new DRAFT with its own ID/content hash and lineage back to the prior artifact.

The model cannot approve itself.

## 3. Destination Normalization

Executable project destinations must be machine-resolvable.

GitHub destinations are normalized to canonical repository identities. Drive destinations must contain an actual Drive resource ID.

Bare names, placeholders, or prose are not executable authority.

## 4. Backend Workspace Readiness

Workspace readiness is determined by backend state, not browser presentation.

For Google Drive, readiness requires:

```text
BuildPrint destination == account drive_destination
AND
drive_ready == true
```

Immediately before execution, Drive access is re-probed using the live interactive Drive token.

For GitHub, read-only access is insufficient; the server must have write authority.

There is no production default workspace and no silent ephemeral fallback.

## 5. Provider Role Resolution

ARCHEMADA resolves three explicit roles:

```text
PLAN
BUILD
VERIFY
```

Planning provenance is read from the BuildPrint. BUILD and VERIFY can use explicit account configuration or inherit PLAN through deterministic application logic.

The model does not choose its own execution provider.

## 6. Native Vertex Capability Check

When BUILD resolves to `vertex_ai`, ARCHEMADA initializes the Vertex execution provider before admission.

Production authentication is runtime Application Default Credentials. No browser API key is required for the Vertex path.

Provider initialization failure is treated as configuration failure before paid execution.

## 7. Materializer Dispatch

The approved repository type deterministically selects the materialization path.

```text
google_drive
    -> Google Drive materializer

existing_repository + canonical GitHub identity
    -> GitHub materializer
```

ARCHESTRATOR receives the prepared local workspace path; it does not decide which remote system owns the project.

## 8. Admission and Billing

The execution boundary is deliberately ordered so configuration failures occur before paid admission.

Conceptually:

```text
workspace readiness
-> live workspace access check
-> provider role resolution
-> provider capability
-> workspace preparation/materialization
-> billing admission
-> RUNNING
```

A bad destination, missing authorization, or unavailable provider should not consume build time simply because BUILD IT was clicked.

## 9. Verification

Verification is not only model opinion.

Concrete deterministic checks can execute against the produced workspace. Model-backed verification may reason over the BuildPrint and result, but deterministic outcomes remain evidence rather than something the model is free to rewrite.

## 10. Durable Completion

For a remote workspace, generating files in an ephemeral execution directory is not durable success.

The approved result must be persisted back to the authorized durable workspace before ARCHEMADA can truthfully represent the remote project as durably complete.

## Why This Matters

ARCHEMADA is not trying to remove autonomy from the software agent.

It is separating **reasoning authority** from **system authority**.

The agent can make substantial engineering decisions inside the approved job while deterministic controls retain authority over identity, lifecycle, workspace, provider resolution, billing, verification evidence, and durable completion.
