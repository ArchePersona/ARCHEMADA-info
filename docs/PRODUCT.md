# ARCHEMADA Product Overview

## Purpose

ARCHEMADA provides a user-facing environment for controlled autonomous software engineering.

Its purpose is not merely to generate code. It is to carry a software request through the engineering work required to turn that request into a reviewable result.

## The Problem

Generating code is only one part of software engineering.

A real engineering job also requires the system to determine what the request means, identify dependencies, recognize decisions that require clarification, establish an execution order, perform the work, verify results, preserve state, and report what happened.

When those responsibilities are hidden inside an opaque model interaction, the user has little basis for understanding how the result was produced or whether the process stayed within the intended scope.

ARCHEMADA is built around that gap.

## Product Model

ARCHEMADA treats each request as an engineering job with an explicit lifecycle.

The user begins with intent rather than implementation detail. The system prepares the job, develops the plan, surfaces consequential decisions, and carries approved work into execution.

The resulting process is intended to remain inspectable from request through result.

## Human Involvement

Different jobs and users require different amounts of interaction during planning.

ARCHEMADA therefore separates the amount of planning involvement from the controls surrounding execution.

A user can choose a more cautious planning experience, a moderate level of autonomy, or a highly autonomous mode. Greater planning autonomy does not mean abandoning verification or lifecycle boundaries.

## Relationship to ARCHESTRATOR

ARCHEMADA is the application. ARCHESTRATOR is the reusable engineering engine underneath it.

That separation keeps user-interface concerns, deployment concerns, and product interaction out of the underlying engineering lifecycle. It also allows ARCHESTRATOR to remain reusable outside a single application.

## Development Status

ARCHEMADA is early-stage software under active development. Interfaces, capabilities, and deployment characteristics may change as the system is hardened.

This repository describes the product at a public level and intentionally omits private implementation material.
