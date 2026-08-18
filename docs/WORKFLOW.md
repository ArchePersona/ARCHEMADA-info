# ARCHEMADA Build Lifecycle

The private `ArchePersona/Archemada` repository is the source of truth for current lifecycle behavior. This document mirrors the externally relevant execution path.

## 1. Intent

The user describes the software they want built.

ARCHEMADA treats that as the beginning of an engineering job, not as immediate permission to generate code.

## 2. Planning

The planning system works through unresolved engineering targets and preserves already-established structured state.

The current default Google path uses Gemini 3.7 Flash through Vertex AI via the Google GenAI SDK.

## 3. BuildPrint DRAFT

Planning produces a BuildPrint DRAFT persisted as account-owned Firestore state.

The BuildPrint contains engineering content, repository/workspace destination, planning provider/model provenance, and a deterministic content hash.

## 4. Approval

Approval is explicit and ownership-checked.

Before DRAFT becomes APPROVED, ARCHEMADA validates the destination and durable-workspace readiness.

Approval is idempotent and does not itself start paid execution.

## 5. Provider Resolution

At the execution boundary, ARCHEMADA resolves effective provider/model roles:

```text
PLAN
BUILD
VERIFY
```

BUILD and VERIFY can be explicitly configured or deterministically inherit PLAN provenance from the approved BuildPrint.

## 6. Workspace Revalidation

Immediately before execution, durable workspace readiness is checked again.

For Google Drive, the BuildPrint Drive resource must match account authority and a live Drive token is used to re-probe access.

For GitHub, the destination must be canonical and server write authority must be available.

## 7. Provider Capability

For the Vertex path, ARCHEMADA initializes the Vertex execution provider before admission.

Production Vertex authentication uses runtime Application Default Credentials rather than a browser API key.

## 8. Materialization

The durable workspace is materialized into an ephemeral local execution directory.

ARCHESTRATOR receives that prepared workspace path instead of owning Drive/GitHub account semantics.

## 9. Billing Admission

Only after the preconditions required for the run are established may execution cross the billing/admission boundary.

The design intent is that workspace/configuration/provider failures occur before build credit is consumed.

## 10. ARCHESTRATOR Execution

ARCHESTRATOR carries the approved engineering job through the bounded build lifecycle using the resolved BUILD provider/model.

## 11. Verification

Verification is a distinct lifecycle phase.

The system can combine deterministic checks with model-backed evaluation. Concrete deterministic outcomes remain authoritative rather than being silently rewritten by model interpretation.

## 12. Writeback

Successful remote-workspace output is synchronized back to the authorized durable workspace.

The temporary execution directory is not the final project authority.

## 13. Completion

For a remote workspace, durable completion requires more than local file generation:

```text
approved work executed
+ verification completed
+ durable writeback succeeded
= durable result
```

BuildPrint/execution linkage and durable application state remain available after the temporary workspace is gone.
