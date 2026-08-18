# ARCHEMADA — All Things Agentic Evaluation

This folder collects evaluation material for ARCHEMADA's **All Things Agentic Hackathon** submission without turning the root public documentation repository into a contest-specific project page.

The root of `ARCHEMADA-info` remains the general public information surface for the product. This folder is the contest/evaluator view.

## Submission Position

ARCHEMADA is a controlled autonomous software-engineering application.

The user describes software, collaborates with a planning agent, approves a durable BuildPrint, and then allows the system to carry the engineering job through materialization, autonomous execution, verification, and durable writeback.

The key distinction is that ARCHEMADA does not treat the chat conversation as the engineering system. The conversation defines the job; explicit application state governs what may execute.

## Mandatory Technology Requirements

The contest requires all submissions to use:

1. Gemini 3.5 or newer through Gemini API or Vertex AI;
2. at least one qualifying Google agent framework/SDK; and
3. at least one Google Cloud infrastructure service.

ARCHEMADA's current implementation maps to those requirements as follows:

| Requirement | ARCHEMADA |
| --- | --- |
| Gemini 3.5+ | Gemini 3.7 Flash |
| Gemini access | Vertex AI |
| Google agent framework / SDK | Google GenAI SDK (`google-genai`) |
| Google Cloud infrastructure | Cloud Run and Firestore |
| Additional Google services | Firebase Authentication, Firebase Hosting, Google Drive API |

The private application package declares `google-genai>=2.0.0` as a runtime dependency and uses the Google/Vertex provider path for model-backed roles.

## What Judges Should Look For

ARCHEMADA is designed around the parts of autonomous software engineering that are easy to hide in a chatbot demo:

- explicit planning state;
- an inspectable BuildPrint;
- human approval before execution;
- durable workspace authority;
- deterministic pre-admission controls;
- autonomous software construction through ARCHESTRATOR;
- verification as a separate lifecycle stage;
- writeback to the user's durable workspace; and
- an execution record that survives the temporary build environment.

## Evaluation Documents

- [Architecture](ARCHITECTURE.md)
- [Deterministic Engine & Control Boundaries](DETERMINISTIC-ENGINE.md)
- [Google Technology Stack](GOOGLE-STACK.md)
- [Project Provenance / New-Project Disclosure](PROVENANCE.md)
- [Demo Evidence Plan](DEMO.md)

## Category Fit

ARCHEMADA's strongest fit is **Taskmaster**: it is intended to complete a multi-step engineering workflow rather than simply return generated text.

A successful build traverses a concrete action path:

```text
intent
-> planning
-> BuildPrint
-> approval
-> workspace materialization
-> autonomous build
-> verification
-> writeback
-> durable result
```

## Public vs. Private Repositories

`ARCHEMADA-info` is public documentation and evaluation material.

The ARCHEMADA implementation repository is private because it contains proprietary source. Contest submission access to the source repository should be provided to the required judging accounts in accordance with the contest rules.

This public repository intentionally does not reproduce private implementation details merely for evaluation convenience.
