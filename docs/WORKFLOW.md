# ARCHEMADA Build Lifecycle

## 1. Intent

The user describes the software they want built in ordinary language.

ARCHEMADA treats that statement as the beginning of an engineering job, not as permission to immediately generate code.

## 2. Planning

The planning agent resolves decisions that materially affect the build.

Examples can include:

- product purpose;
- target users;
- required capabilities;
- integrations;
- data requirements;
- authentication/access expectations;
- deployment constraints;
- acceptance criteria; and
- durable project destination.

Already-established structured state is authoritative. If a durable workspace has already been selected and authorized, the planner should not re-ask where the project lives.

## 3. BuildPrint Draft

When planning is sufficiently resolved, ARCHEMADA creates a BuildPrint DRAFT.

The BuildPrint records the engineering plan and project destination as durable application state.

The user can inspect the draft before execution.

## 4. Approval

Approval is explicit.

Before a DRAFT becomes APPROVED, ARCHEMADA checks that the project destination is machine-resolvable and that the durable workspace is ready.

Approval does not spend build time and does not automatically mutate the plan.

## 5. Workspace Revalidation

Immediately before execution, ARCHEMADA revalidates the durable workspace.

For Google Drive, the selected resource must match the approved BuildPrint destination and the live authorization must be sufficient to read the workspace.

## 6. Materialization

The durable remote workspace is materialized into a unique ephemeral execution directory.

ARCHESTRATOR receives that local workspace path rather than provider-specific Drive or GitHub logic.

## 7. Provider Capability

ARCHEMADA resolves the effective BUILD provider/model and checks capability before paid admission.

Current role resolution supports explicit BUILD configuration or inheritance from PLAN.

## 8. Admission

Only after workspace and provider preconditions succeed does the execution cross the billing/admission boundary.

The run then becomes a real execution record.

## 9. ARCHESTRATOR Execution

ARCHESTRATOR carries the approved engineering job forward inside the materialized workspace.

The lifecycle is bounded by the BuildPrint, execution environment, provider configuration, and system controls established before admission.

## 10. Verification

The produced software is checked before durable completion.

Verification can include deterministic project checks and model-backed assessment against the approved BuildPrint.

A failed verification remains a failed verification; model reasoning does not silently convert a concrete failure into success.

## 11. Writeback

Successful output is synchronized back to the authorized durable workspace.

For Drive, ARCHEMADA compares source version identity where supported so remote drift can block unsafe overwrite.

## 12. Completion

A remote-workspace build is not considered durably complete merely because code exists in the ephemeral execution directory.

The intended completion condition is:

```text
approved work executed
+ verification completed
+ writeback succeeded
= durable result
```

Execution records and provenance remain available after the temporary workspace is cleaned up.
