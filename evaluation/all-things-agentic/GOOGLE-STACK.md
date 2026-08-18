# Google Technology Stack

## Mandatory Hackathon Stack

ARCHEMADA's current implementation satisfies the three mandatory technology categories through the following stack:

| Hackathon requirement | ARCHEMADA implementation |
| --- | --- |
| Gemini 3.5 or newer | Gemini 3.7 Flash |
| Gemini access path | Vertex AI |
| Google agent framework / SDK | Google GenAI SDK (`google-genai`) |
| Google Cloud infrastructure | Cloud Run + Firestore |

Additional Google services include Firebase Authentication, Firebase Hosting, and Google Drive APIs.

## Google GenAI SDK

The ARCHEMADA Python application declares:

```text
google-genai>=2.0.0
```

as a runtime dependency.

The SDK is used in the Google model provider path rather than being included only as an unused contest dependency.

## Vertex AI + Gemini

The current primary Google model path uses Gemini 3.7 Flash through Vertex AI.

ARCHEMADA represents model usage by role:

```text
PLAN
BUILD
VERIFY
```

This allows model provenance to remain explicit in the engineering lifecycle. When BUILD or VERIFY does not specify an override, the role can inherit the PLAN provider/model through deterministic application configuration.

## Cloud Run

Cloud Run provides the server-side ARCHEMADA application/execution boundary.

The FastAPI service handles planning requests, authenticated state operations, BuildPrint lifecycle, workspace validation/materialization, execution admission, and execution coordination.

Cloud Run's temporary local filesystem is treated as execution space, not as durable project storage.

## Firestore

Firestore provides durable state for the application layer.

Examples include:

- account/provider configuration;
- BuildPrint records and lifecycle;
- selected workspace authority/readiness; and
- execution-related records.

This keeps engineering state outside the chat transcript and outside temporary container memory.

## Firebase Authentication

Firebase Authentication provides the application's Google identity boundary.

The Firebase ID token authenticates the user to ARCHEMADA's backend.

This is intentionally separate from Google Drive authorization.

## Firebase Hosting

Firebase Hosting serves the ARCHEMADA browser application.

The browser is the user's control surface for planning, BuildPrint review, workspace selection, provider configuration, and execution visibility.

## Google Drive API

Google Drive can act as a durable project workspace.

ARCHEMADA uses the narrow `drive.file` authorization scope for resources the user selects or creates through the application flow.

The Drive OAuth access token is treated as authorization for Drive operations and remains distinct from the Firebase ID token used for ARCHEMADA identity.

A Drive-backed build follows this pattern:

```text
selected Drive folder
-> backend readiness proof
-> materialize into ephemeral workspace
-> autonomous build
-> verification
-> drift-aware writeback
-> durable Drive result
```

## Proof in the Demo

The contest demo should visibly establish the Google stack rather than relying only on documentation.

Useful evidence includes:

- ARCHEMADA showing the effective Gemini/Vertex model role;
- Cloud Run service/revision or execution logs;
- Firestore state changing for the BuildPrint/execution;
- the selected Google Drive workspace;
- files written back to that Drive workspace after execution; and
- the architecture diagram showing where the Google services participate.

The demo does not need to expose credentials, tokens, or private source code in order to prove those integrations.
