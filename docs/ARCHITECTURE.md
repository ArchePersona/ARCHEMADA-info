# ARCHEMADA Architecture

## Public Architecture Overview

ARCHEMADA is intentionally thin at the application boundary.

The application is responsible for the user-facing engineering workflow and delegates the reusable engineering lifecycle to ARCHESTRATOR.

```text
User
  |
  v
ARCHEMADA
  |  request / planning / approval / inspection
  v
ARCHESTRATOR
  |  bounded engineering lifecycle
  v
Engineering Work + Verification + Result
```

This document describes only the public architectural boundary. Internal implementation details, private control mechanisms, credentials, deployment secrets, and non-public interfaces are intentionally excluded.

## ARCHEMADA Responsibilities

At a high level, ARCHEMADA owns:

- the browser-facing application experience;
- the application API boundary;
- job preparation and lifecycle interaction;
- presentation of planning decisions;
- user approval interaction;
- lifecycle-result inspection; and
- application deployment concerns.

## ARCHESTRATOR Boundary

ARCHESTRATOR provides the engineering lifecycle consumed by ARCHEMADA.

The separation is deliberate. ARCHEMADA should not duplicate the engine's engineering semantics, and ARCHESTRATOR should not become coupled to one application's presentation layer.

## Dependency Model

The current ARCHEMADA application consumes a pinned ARCHESTRATOR build. This provides a defined engine revision for a given application build rather than silently depending on a moving upstream target.

## Design Principle

The principal architectural rule is separation of responsibility.

The user-facing application should be able to evolve without rewriting the engineering engine, while the engineering engine should be able to evolve without absorbing application-specific presentation concerns.

## Wider System

ARCHEMADA exists within ARCHETRON, where other components have separate responsibilities for areas such as evidence, observation, telemetry, governance, and attention.

Those responsibilities remain separate from ARCHEMADA's application boundary. This public document intentionally does not describe internal cross-system protocols or private implementation topology.
