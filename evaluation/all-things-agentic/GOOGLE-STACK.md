# Google Technology Stack

## Source of Truth

This evaluation document is derived from the private `ArchePersona/Archemada` implementation repository. The implementation repository is authoritative for current behavior.

## Mandatory Hackathon Stack

ARCHEMADA's current implementation satisfies the mandatory technology categories through the following stack:

| Hackathon requirement | ARCHEMADA implementation |
| --- | --- |
| Gemini 3.5 or newer | Gemini 3.7 Flash |
| Gemini access path | Vertex AI |
| Google agent framework / SDK | Google GenAI SDK (`google-genai>=2.0.0`) |
| Google Cloud infrastructure | Cloud Run + Firestore |

Additional Google services include Firebase Authentication, Firebase Hosting, and Google Drive APIs.

## Google GenAI SDK

The private Python application declares:

```text
google-genai>=2.0.0
```

The SDK is part of the actual Vertex provider path. It is not present only as a contest dependency.

The planning adapter initializes:

```text
genai.Client(
    vertexai=True,
    project="archemada",
    location="global"
)
```

and invokes Gemini with:

```text
model="gemini-3.7-flash"
```

## Vertex AI Authentication

Production Vertex authentication uses Application Default Credentials from the Cloud Run runtime identity.

The Vertex path does not require a browser API key or BYOK credential.

## Provider Roles

ARCHEMADA represents model usage through explicit roles:

```text
PLAN
BUILD
VERIFY
```

The approved BuildPrint carries planning provider/model provenance. BUILD and VERIFY can use explicit configuration or inherit the PLAN provider/model through deterministic application logic.

## Cloud Run

Cloud Run provides the backend/execution service boundary.

The FastAPI application handles planning requests, authenticated state operations, BuildPrint lifecycle, workspace readiness, execution initiation, provider capability checks, and build coordination.

Cloud Run local storage is treated as temporary execution space, not durable project authority.

## Firestore

Firestore provides durable application state, including account/provider configuration, BuildPrint records and lifecycle, workspace authority/readiness, and execution linkage/state.

BuildPrint state is account-owned and ownership-checked server-side.

## Firebase Authentication

Firebase Authentication provides account identity to the ARCHEMADA backend.

Firebase identity is intentionally distinct from Google Drive authorization.

## Firebase Hosting

Firebase Hosting serves the browser application used for planning, BuildPrint control, workspace selection, provider settings, and execution visibility.

## Google Drive API

Google Drive can act as the durable project authority.

A Drive workspace is READY only when the selected resource matches backend account authority and has been backend-verified. Before execution, Drive access is re-probed using the live interactive Drive authorization.

The current production contract has no unnamed default workspace and no silent ephemeral fallback.

## Demo Proof

The demo should visibly prove the stack rather than relying only on this document:

- Gemini 3.7 Flash / Vertex provider identity;
- Google GenAI SDK in the implementation dependency/provider path;
- Cloud Run service/revision or execution logs;
- Firestore-backed BuildPrint/execution state;
- the selected Google Drive workspace; and
- durable writeback of generated software to that workspace.

Credentials, OAuth tokens, and private source code do not need to be exposed to prove these integrations.
