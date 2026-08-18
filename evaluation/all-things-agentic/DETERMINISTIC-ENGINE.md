# Deterministic Engine & Control Boundaries

## Why Deterministic Control Exists

ARCHEMADA uses generative models for the work models are good at: interpreting intent, planning, constructing software, and evaluating complex outcomes.

It does not ask the model to own every consequential system decision.

Several boundaries remain deterministic because they define authority, state, billing, persistence, or whether execution is allowed to begin at all.

## Model Reasoning vs. System Authority

A useful distinction is:

```text
MODEL:
What should this software do?
How should the implementation satisfy the BuildPrint?
What changes are required?
Does the produced result satisfy higher-level requirements?

SYSTEM:
Which BuildPrint is active?
Has the user approved it?
Where is the authorized project workspace?
Is that workspace actually ready?
Which provider/model is effective for this stage?
May paid execution begin?
Did deterministic verification pass?
Did durable writeback succeed?
```

The model reasons within the engineering job. The application controls the boundary around the job.

## 1. Planning Target State

Planning targets have explicit application state such as resolved, partial, unresolved, or irrelevant.

This matters because facts already established by the application should not be repeatedly re-decided by the model. A selected durable workspace, for example, can be supplied as structured state and marked resolved before the planning request reaches Gemini.

## 2. Choice Resolution

When ARCHEMADA presents explicit numbered options, simple replies can be resolved deterministically against the active question.

Example:

```text
1. GitHub repository
2. Google Drive workspace

User: 2
```

The application can resolve `2` to the semantic option before model interpretation. It does not globally reinterpret arbitrary numbers; the mapping is scoped to an active option set.

This prevents a UI choice from depending on the model reconstructing what a bare number meant.

## 3. BuildPrint Lifecycle

BuildPrint state is application-owned.

Typical lifecycle:

```text
DRAFT -> APPROVED
  |         |
  +-------> CANCELLED
```

Approval is explicit and account-scoped. Revision creates a new draft rather than silently rewriting historical approved content.

The model does not grant approval to itself.

## 4. Destination Normalization

Durable project locations must be machine-resolvable.

A GitHub destination must resolve to a canonical repository identity. A Google Drive destination must contain an actual resource ID rather than prose such as `selected_drive_workspace`.

Placeholders and conversational references are not accepted as executable project authority.

## 5. Workspace Readiness

The browser cannot declare a workspace READY merely because a label or selection exists in the UI.

Readiness is established through the backend against account-owned configuration and the actual destination.

For Drive, identity and Drive authorization remain distinct. Firebase establishes ARCHEMADA account identity; the Google OAuth token authorizes Drive operations.

## 6. Materializer Dispatch

The approved destination is deterministically mapped to the correct workspace adapter.

Examples:

```text
existing_repository + canonical GitHub location
    -> GitHub materializer

google_drive + Drive resource ID
    -> Google Drive materializer
```

ARCHESTRATOR receives the resulting local path. It does not choose which remote provider the project came from.

## 7. Provider Inheritance

PLAN, BUILD, and VERIFY are explicit roles.

If BUILD or VERIFY has no independent override, the application deterministically resolves that role to the PLAN provider/model. The model does not choose its own provider during the run.

## 8. Admission and Billing

ARCHEMADA establishes required preconditions before paid execution begins.

The intended ordering is:

```text
workspace readiness
-> materialization
-> provider capability
-> billing admission
-> RUNNING
```

A repository typo, missing Drive authorization, or materialization failure should not become paid model runtime merely because the user clicked BUILD IT.

## 9. Deterministic Verification

Verification is not only a model opinion.

Concrete project checks can be executed deterministically. Their outputs become evidence for the execution record and, where model-backed verification is also used, evidence the model may interpret.

A concrete failed check remains a failed check.

## 10. Writeback Completion

For remote workspaces, persistence success is a deterministic completion requirement.

Producing files inside the temporary execution directory is not sufficient. Successful output must reach the approved durable destination before the system can truthfully claim a durable result.

Where supported, remote version identity is rechecked before writeback so source drift can block unsafe overwrite.

## Why This Matters for Autonomous Agents

The goal is not to reduce model autonomy to a script.

The goal is to place autonomy inside explicit, inspectable constraints so the agent can do meaningful work without also being the sole authority over identity, money, destination, lifecycle, and truth.

ARCHEMADA's deterministic engine is therefore less about replacing reasoning and more about **constraining where reasoning is allowed to become action**.
