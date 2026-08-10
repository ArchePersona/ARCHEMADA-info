# ARCHEMADA

**Tell it what you want built. Work through the plan. Let the system carry the build forward.**

ARCHEMADA is a software engineering application that turns a software request into a controlled, inspectable engineering process.

Rather than treating software development as a single prompt-and-response interaction, ARCHEMADA organizes the work around the engineering job itself: preparation, planning, approval, execution, verification, and review.

> This repository contains public product information and documentation for ARCHEMADA. It does not contain the application source code or private implementation details.

## What ARCHEMADA Does

A typical ARCHEMADA workflow is:

1. Describe what you want built.
2. Let the system prepare the engineering job.
3. Resolve planning decisions that materially affect the work.
4. Review and approve the plan.
5. Execute the build through a bounded engineering lifecycle.
6. Inspect the work, checks, results, and final record.

The objective is straightforward: make autonomous software engineering easier to understand, supervise, and trust.

## Engineering Jobs, Not Prompt Chains

Most AI coding interfaces center the conversation. ARCHEMADA centers the engineering job.

The conversation helps define the work, but the durable objects are the plan, lifecycle state, execution record, verification results, and produced software.

This allows the system to carry substantial work forward without making the process invisible.

## Planning and Human Involvement

ARCHEMADA is designed to ask for human input when an answer materially changes the build while allowing routine decisions to proceed without unnecessary interruption.

Planning can support different levels of involvement:

- **Cautious** — ask about most meaningful choices.
- **Moderate** — ask about major choices and make safe assumptions on smaller ones.
- **Trust Me, Bro** — make most planning choices and stop for genuine blockers.

These modes describe planning involvement. They do not remove execution checks, verification, or system controls.

## Architecture Boundary

ARCHEMADA is intentionally an application layer rather than a monolithic engineering engine.

It owns the user-facing engineering experience, application boundary, lifecycle interaction, and presentation of the work.

The reusable engineering lifecycle beneath ARCHEMADA is provided by **ARCHESTRATOR**.

Keeping those responsibilities separate allows the application experience and the engineering engine to evolve independently.

## Current Technical Profile

The current application is implemented as a Python application with:

- a browser-based interface;
- a FastAPI service boundary;
- preparation and lifecycle endpoints;
- a bounded engineering lifecycle;
- lifecycle-result inspection;
- deployment support for Render; and
- a pinned ARCHESTRATOR dependency.

ARCHEMADA is currently early-stage software under active development. Public documentation should not be interpreted as a stability or production-readiness guarantee.

## Documentation

- [Product Overview](docs/PRODUCT.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Security](SECURITY.md)
- [Support](SUPPORT.md)
- [License](LICENSE)

## ARCHETRON

ARCHEMADA is part of the broader ARCHETRON system, whose components deliberately separate responsibilities such as engineering, evidence, observation, telemetry, governance, and attention.

ARCHEMADA's responsibility is the software-engineering application experience. It does not need to absorb the responsibilities of those other systems in order to perform that job.

## Repository Scope

This is an information repository. It is intended for public-facing product documentation, evaluation material, technical orientation, and links that can be shared without exposing the private application repository.

No source-code license is granted by the presence of this repository.

---

Copyright © 2026 ARCHETRON. All rights reserved.
