# Demo Evidence Plan

## Goal

The demo should prove that ARCHEMADA performs autonomous software-engineering work rather than merely generating conversational text.

The strongest evidence is one continuous execution from user intent to durable software output.

## Four-Minute Story

### 1. The Friction

Open with the problem in one sentence:

> AI can generate code, but a user still needs a trustworthy way to turn intent into an approved engineering job, execute it autonomously, verify the result, and preserve the software somewhere durable.

### 2. Plan

Show ARCHEMADA receiving a concrete software request.

Show the planning agent resolving only consequential decisions and producing the BuildPrint.

Evidence to capture:

- Gemini/Vertex is the active planning path;
- the selected durable workspace is already known when applicable;
- the BuildPrint records the objective and destination.

### 3. Approval

Open the BuildPrint.

Show that the artifact is DRAFT before approval and APPROVED afterward.

This is important because it demonstrates that execution is not an uncontrolled continuation of the conversation.

### 4. Autonomous Action

Click **BUILD IT** once.

Show the execution surface while the build proceeds without manual coding.

Useful visible evidence:

- execution ID/state;
- effective provider/model;
- ARCHESTRATOR execution activity;
- verification activity;
- concrete files/actions rather than only model prose.

### 5. Google Cloud Proof

The contest explicitly asks for proof that the backend is running on Google Cloud.

Include a short cut showing one or more of:

- Cloud Run service/revision;
- Cloud Run logs for the execution;
- Vertex AI request/activity evidence;
- Firestore record/state;
- the deployed Firebase-hosted application.

Do not spend the demo reading infrastructure screens. A few seconds of unmistakable proof is enough.

### 6. Durable Result

End by opening the selected Google Drive workspace and showing the generated software files present after the execution completes.

This proves that ARCHEMADA did not merely produce code inside a temporary Cloud Run filesystem.

The final visual should establish:

```text
User intent
-> autonomous engineering action
-> verified result
-> persisted software
```

## What the Demo Should Not Do

Do not spend the limited time:

- explaining every internal subsystem;
- navigating source code line-by-line;
- showing setup/configuration longer than necessary;
- presenting a fake or pre-recorded terminal as if it were the live run;
- repeatedly retrying a failed build on camera; or
- claiming completion before Drive writeback is visibly proven.

## Architecture Explanation

Use the architecture diagram to explain the system in roughly this order:

```text
Browser / Firebase
        ↓
Cloud Run application
        ↓
Gemini planning
        ↓
BuildPrint + Firestore
        ↓
Approval / deterministic controls
        ↓
Workspace materialization
        ↓
ARCHESTRATOR + Gemini build
        ↓
Verification
        ↓
Drive writeback
```

The point is not that ARCHEMADA contains many boxes. The point is that the boxes enforce separation of concerns around autonomous action.

## Deterministic Engine Talking Point

A concise explanation for the video:

> Gemini reasons about the software. ARCHEMADA deterministically controls the authority around that reasoning — which plan is approved, which workspace is authorized, when paid execution can begin, what verification actually passed, and whether the result was durably written back.

## Submission Evidence Checklist

Before recording the final demo, capture/verify:

- [ ] clean BuildPrint with real durable workspace ID;
- [ ] BuildPrint approval succeeds;
- [ ] BUILD IT starts the exact approved artifact;
- [ ] effective model/provider is visible;
- [ ] Cloud Run backend evidence is available;
- [ ] Firestore execution/BuildPrint evidence is available;
- [ ] build reaches a truthful terminal state;
- [ ] verification result is visible;
- [ ] Drive writeback succeeds;
- [ ] generated software is visibly present in Drive;
- [ ] no credentials/API keys appear in the recording.

## Documentation Evidence

The final submission should also point evaluators to:

- the public product overview;
- the architecture document;
- the deterministic-engine explanation;
- the Google stack mapping;
- the pre-existing ARCHESTRATOR disclosure; and
- reproducible setup/deployment instructions in the source repository README.
