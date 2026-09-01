# EPA Digital — KC-1299 Control Execution Platform

> **Note:** This project is in early stages. The scope, workflow and architecture described below are based on initial analysis and may change significantly as the project evolves.

> EPA Digital is not intended to replace every analyst calculation model. Instead, it provides a controlled environment for reviewing EPA outputs, recording business decisions, executing KC-1299 and maintaining audit-ready evidence.

## Overview

EPA Digital is a Databricks application designed to support and digitalise the execution of **KC-1299 Commodity Peer Review**, a key control within Equinor's Energy Perspectives Analysis (EPA) process.

The solution replaces large parts of the current Excel-based control execution process by providing a structured workflow for:

* Reviewing default EPA calculation results
* Capturing analyst decisions and business judgement
* Managing approvals and control execution
* Maintaining a complete audit trail
* Generating evidence required for compliance and review
* Producing an immutable control package

The first implementation focuses on the **Power Pilot**, which will serve as the reference implementation for future commodities and markets.

## Business Problem

Today, the KC-1299 process relies heavily on:

* Excel orchestrator workbooks
* Manual review activities
* Screenshots and supporting documentation
* Copy/paste operations between systems
* Manual sign-off procedures
* Audit evidence stored across multiple locations

This makes the process:

* Time consuming
* Difficult to trace
* Dependent on manual documentation
* Challenging to audit
* Hard to scale across commodities and geographies

EPA Digital aims to establish a controlled, transparent and repeatable process where all manual decisions, approvals and evidence are captured directly in the system.

## Core Concept

The application is built around a **Control Run**.

A control run represents the execution of a KC-1299 review cycle for a given quarter.

Example:

| Field      | Value     |
| ---------- | --------- |
| Control    | KC-1299   |
| Quarter    | Q3 2026   |
| Commodity  | Power     |

Each control run progresses through a defined workflow from creation to final approval and evidence locking.

## Workflow

### Stage 1 — Control Run Created

The EPA Coordinator initiates a new control run containing:

* Quarter
* Control ID
* Atlas run reference
* Assigned analysts
* EPA Coordinator
* Due date

### Stage 2 — Default EPA Results Available

Atlas provides a baseline EPA run. The application loads:

* Benchmark datasets
* Default EPA outputs
* Short-term prices
* Current approved calculations

These values represent the starting point for review.

### Stage 3 — Analyst Review and Business Decisions

Commodity analysts review the default results. Analysts may accept the baseline values or introduce controlled business decisions where required.

Examples:

* Selecting an alternative inflation series
* Changing a rolling window used for short-term prices
* Applying commodity-specific business judgement

Whenever a manual decision is made, the application captures:

* Selected value
* Previous value
* Reason for change
* Analyst
* Timestamp
* Analyst sign-off

This becomes part of the control evidence.

### Stage 4 — EPA Coordinator Review

The EPA Coordinator reviews:

* Analyst decisions
* Supporting rationale
* Evidence collected
* Impact to calculated outputs

The Coordinator can:

* Return items to the analyst
* Request clarification
* Continue the control process

### Stage 5 — Approval

The control is formally executed. Approval confirms:

* Review completed
* Evidence requirements satisfied
* Required sign-offs completed

The approved version becomes the official control outcome.

### Stage 6 — Evidence Locked

The application generates and stores an immutable control package containing:

* Baseline Atlas results
* Manual decisions
* Change rationale
* Sign-offs
* Approval history
* User activity history
* Timestamps
* Supporting evidence

Once locked, the package becomes the historical record of the control execution.

## User Roles

### Commodity Analyst

Responsible for:

* Reviewing EPA outputs
* Making business decisions when necessary
* Providing justification for changes
* Completing analyst review

### EPA Coordinator

Responsible for:

* Creating control runs
* Reviewing analyst decisions
* Verifying evidence
* Approving control execution

### Auditor (Future)

Responsible for:

* Reviewing completed evidence packages
* Verifying compliance requirements
* Inspecting historical control runs

Auditor access requirements are currently under clarification.

## Key Principles

### Baseline First

Atlas provides the default EPA results. Analysts should only deviate from the baseline where business judgement is required.

### Evidence by Design

All manual actions are automatically captured. Evidence is generated as a natural result of using the application rather than through manual screenshot collection.

### Traceability

Every business decision must be traceable. The system records:

* Who performed an action
* When the action occurred
* What was changed
* Why the change was made

### Controlled Overrides

Manual changes are allowed but must always include rationale and sign-off. No change should occur without evidence.

### Immutable History

Approved control runs are preserved as historical records and cannot be modified.

## Future Scope

The Power Pilot establishes the foundation for:

* Additional commodity groups
* Additional countries
* Expanded EPA calculations
* Enhanced reporting
* Auditor self-service access
* Automated compliance reporting

The workflow, approval model and evidence framework developed in the Power Pilot are expected to be reusable across future EPA Digital implementations.

## High-Level Architecture

```
Atlas Data Products
        │
        ▼
Default EPA Results
        │
        ▼
    EPA Digital
        │
        ├── Analyst Review
        ├── Manual Decisions
        ├── Evidence Collection
        ├── Coordinator Review
        └── Approval Workflow
        │
        ▼
Immutable Control Package
        │
        ▼
Audit & Compliance Review
```