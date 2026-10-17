---
name: salesforce-flow-design
description: "Design or review Salesforce Flows for flow-type selection, bulk safety, recursion, fault handling, and transaction boundaries. Use for Flow requirements or Flow XML review; exclude diagram-only requests and deployment execution."
license: MIT
metadata:
  category: salesforce-certinia
  source: https://github.com/github/awesome-copilot/tree/cb64ad12e32737ae764becd6885462e9b4c7cb00/skills/salesforce-flow-design
  author: "GitHub, Inc. and contributors"
  upstream-commit: "cb64ad12e32737ae764becd6885462e9b4c7cb00"
---

# Salesforce Flow Design and Validation

Local modification (2026-10-17): adapted for portable, offline design review; corrected transaction, invocation, fault, and termination guidance.

Use supplied requirements, Flow XML, and the relevant automation inventory. No CLI or org is required for local design review. If inputs are missing, identify them and provide a provisional design; never invent an org inventory.

Apply these checks when the task is Flow design or review. Diagram-only requests belong to `sf-diagram`; holistic architecture reviews and actual deployments are outside this skill. Deliver a flow-type recommendation, evidence with element names or file locations, corrections, and a nonproduction test plan.

Deployment, activation, deactivation, record changes, and sending emails require explicit authorization for the target environment and action. This skill does not perform them implicitly.

## Step 1 — Confirm Flow Is the Right Tool

Before designing a Flow, verify that a lighter-weight declarative option cannot solve the problem:

| Requirement | Best tool |
|---|---|
| Calculate a field value with no side effects | Formula field |
| Prevent a bad record save with a user message | Validation rule |
| Sum or count child records on a parent | Roll-up Summary field where the relationship supports it |
| Complex logic or high volume | Evaluate Flow, invocable Apex, or Apex against volume, transaction requirements, and existing automation |
| Everything else | Flow ✓ |

When a formula or validation rule meets the requirement, explain that simpler option and preserve the requested scope.

## Step 2 — Select the Correct Flow Type

| Use case | Flow type | Key constraint |
|---|---|---|
| Update a field on the same record before it is saved | Before-save Record-Triggered | Cannot send emails, make callouts, or change related records |
| Create/update related records, emails, callouts | After-save Record-Triggered | Immediate path runs before transaction commit; use an appropriate asynchronous path for after-commit work |
| Guide a user through a multi-step UI process | Screen Flow | Cannot be triggered by a record event automatically |
| Reusable background logic called from another Flow | Autolaunched (Subflow) | Input/output variables define the contract |
| Background Flow invoked from Apex | Autolaunched | Define input/output variables; `@InvocableMethod` instead exposes Apex actions for Flow to call |
| Time-based batch processing | Scheduled Flow | Runs in batch context — respect governor limits |
| Respond to a supported Platform Event | Platform Event–Triggered | Runs asynchronously — eventual consistency |

**Decision rule**: choose before-save when you only need to change the triggering record's own fields. Use after-save for related-record work; check the supported asynchronous or scheduled path and transaction boundary before introducing callouts.

## Step 3 — Bulk Safety Checklist

These patterns are governor limit failures at scale. Check for all of them before the Flow is activated.

### DML in Loops — Automatic Fail

```
Loop element
  └── Create Records / Update Records / Delete Records  ← ❌ DML inside loop
```

Fix: collect records inside the loop into a collection variable, then run the DML element **outside** the loop.

### Get Records in Loops — Automatic Fail

```
Loop element
  └── Get Records  ← ❌ SOQL inside loop
```

Fix: perform the Get Records query **before** the loop, then loop over the collection variable.

### Correct Bulk Pattern

```
Get Records — collect all records in one query
└── Loop over the collection variable
    └── Decision / Assignment (no DML, no Get Records)
└── After the loop: Create/Update/Delete Records — one DML operation
```

### Transform vs Loop
When the goal is reshaping a collection (e.g. mapping field values from one object to another), use the **Transform** element instead of a Loop + Assignment pattern. Transform is bulk-safe by design and produces cleaner Flow graphs.

## Step 4 — Fault Path Requirements

Add fault connectors to fallible elements that support them, and define the intended rollback or recovery behavior. Flows without fault paths surface raw system errors to users.

### Fault and missing-result checks
- Create Records
- Update Records
- Delete Records
- After Get Records, use a Decision to handle an empty result; a missing record is not itself a fault
- Send Email
- HTTP Callout / External Service action
- Apex action (invocable)
- Subflow (if the subflow can throw a fault)

### Fault Handler Pattern
```
Fault connector → Log Error (Create Records on a logging object or fire a Platform Event)
               → Screen element with user-friendly message (Screen Flows)
               → Intentional termination or supported error/rollback path (Record-Triggered Flows)
```

Never connect a fault path back to the same element that faulted — this creates an infinite loop.

## Step 5 — Automation Density Check

Before deploying, inventory overlapping automations on the same object and trigger event and check their intended execution order:

- Other active Record-Triggered Flows on the same `Object` + `When to Run` combination
- Legacy Process Builder rules still active on the same object
- Workflow Rules that fire on the same field changes
- Apex triggers that also run on the same `before insert` / `after update` context

Overlapping automations can cause unexpected ordering, recursion, and governor limit failures. Document the automation inventory for the object before activating.

## Step 6 — Screen Flow UX Guidelines

- Every Screen Flow path must terminate or navigate intentionally; End is a diagram convention, not a required XML element.
- Provide a **Back** navigation option on multi-step flows unless back-navigation would corrupt data.
- Use appropriate standard Flow screen components; custom Flow screen LWCs need accessible labels, validation, and keyboard behavior.
- Validate required inputs on the screen before the user can advance — use Flow validation rules on the screen.
- Check whether pause/resume is supported for the selected Flow type and interview context before relying on it.

## Step 7 — Deployment Safety

```
Deploy as Draft    →   Test with 1 record   →   Test with 200+ records   →   Activate
```

- Plan deployment as **Draft** and nonproduction validation before activation; execution needs explicit authorization.
- For Record-Triggered Flows: test with the exact entry conditions (e.g. `ISCHANGED(Status)` — ensure the test data actually triggers the condition).
- For Scheduled Flows: test with a small batch in a sandbox before enabling in production.
- Assess automation density using volume, number of automations, and downstream dependency depth; do not invent a universal numeric cutoff.

## Quick Reference — Flow Anti-Patterns Summary

| Anti-pattern | Risk | Fix |
|---|---|---|
| DML element inside a Loop | Governor limit exception | Move DML outside the loop |
| Get Records inside a Loop | SOQL governor limit exception | Query before the loop |
| No fault connector on DML/email/callout element | Unhandled exception surfaced to user | Add fault path to every such element |
| Updating the triggering record in an after-save flow with no recursion guard | Infinite trigger loops | Add an entry condition or recursion guard variable |
| Treating `$Record` as a collection | Incorrect data contract | `$Record` represents the triggering record; build collections explicitly where needed |
| Process Builder still active alongside a new Flow | Double-execution, unexpected ordering | Plan and authorize the migration cutover after regression testing |
| Orphan or unintended Screen Flow path | Missing user outcome | Check connectors and intentional path termination |

For platform details, consult the current [record-triggered automation guide](https://architect.salesforce.com/docs/architect/decision-guides/guide/record-triggered) and [Flow security guidance](https://developer.salesforce.com/docs/platform/secure-coding/guide/secure-coding-considerations-flow-design.html). Check the project API version and feature availability.
