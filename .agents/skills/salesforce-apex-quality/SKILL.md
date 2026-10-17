---
name: salesforce-apex-quality
description: "Review or author Salesforce Apex classes, triggers, and tests for bulk safety, sharing, CRUD/FLS, injection, and meaningful tests. Use for Apex source quality; runtime-log diagnosis belongs to platform-apex-logs-debug."
license: MIT
metadata:
  category: salesforce-certinia
  source: https://github.com/github/awesome-copilot/tree/cb64ad12e32737ae764becd6885462e9b4c7cb00/skills/salesforce-apex-quality
  author: "GitHub, Inc. and contributors"
  upstream-commit: "cb64ad12e32737ae764becd6885462e9b4c7cb00"
---

# Salesforce Apex Quality Guardrails

Local modification (2026-10-17): adapted for scoped authoring/review and version-aware access checks; corrected unsafe security guidance and an invalid query example.

Use supplied Apex, tests, class metadata API versions, and acceptance criteria. Local review requires no org or CLI. Without source or access, explain the missing evidence and provide a provisional checklist. Do not fabricate analyzer, compilation, coverage, or test results.

Apply these checks when the user requests Apex authoring or code review. Diagnose supplied runtime logs with `platform-apex-logs-debug`; standalone query design, permission administration, and whole-project architecture grading are separate tasks. A review request produces findings; edit only when authoring or fixes are requested. Preserve the project's framework and unrelated work.

Deliver prioritized findings with file/line evidence, proposed changes, and a positive/negative/bulk test plan. Live test runs, anonymous Apex, deployments, and org changes require explicit authorization for the target and operation.

## Step 1 — Governor Limit Safety Check

Scan for these patterns before declaring any Apex file acceptable:

### SOQL and DML in Loops — Automatic Fail

```apex
// ❌ NEVER — causes LimitException at scale
for (Account a : accounts) {
    List<Contact> contacts = [SELECT Id FROM Contact WHERE AccountId = :a.Id]; // SOQL in loop
    update a; // DML in loop
}

// ✅ ALWAYS — collect, then query/update once
Set<Id> accountIds = new Map<Id, Account>(accounts).keySet();
Map<Id, List<Contact>> contactsByAccount = new Map<Id, List<Contact>>();
for (Contact c : [SELECT Id, AccountId FROM Contact WHERE AccountId IN :accountIds]) {
    if (!contactsByAccount.containsKey(c.AccountId)) {
        contactsByAccount.put(c.AccountId, new List<Contact>());
    }
    contactsByAccount.get(c.AccountId).add(c);
}
update accounts; // DML once, outside the loop
```

Rule: if you see `[SELECT` or `Database.query`, `insert`, `update`, `delete`, `upsert`, `merge` inside a `for` loop body — trace the execution path and recommend collection-based work. A SOQL for-loop iterates one query result and is not itself a query-per-iteration defect.

## Step 2 — Sharing Model Verification

Make class sharing intent explicit and evaluate caller, entry point, and API version. Omitted sharing behavior is context/version dependent. Triggers have special execution rules and do not take class sharing declarations.

| Declaration | When to use |
|---|---|
| `public with sharing class Foo` | Default for all service, handler, selector, and controller classes |
| `public without sharing class Foo` | Only when the class must run elevated (e.g. system-level logging, trigger bypass). Requires a code comment explaining why. |
| `public inherited sharing class Foo` | Framework entry points that should respect the caller's sharing context |

If a class omits sharing intent, report the relevant risk; choose a declaration only after checking the intended access contract. Do not silently change privilege semantics during review.

## Step 3 — CRUD / FLS Enforcement

Treat record sharing and object/field permissions as separate controls. `with sharing` does not enforce CRUD/FLS. Inspect the class API version and explicit operation access mode rather than assuming a universal default. Current platform guidance distinguishes API 67.0+ user-mode defaults from earlier system-mode defaults; triggers need separate analysis.

For user-scoped data, prefer explicit user-mode queries and DML where supported. If graceful removal of inaccessible fields is intended, consider `Security.stripInaccessible()`, inspect removed fields, and preserve the operation's semantics. Document and justify any system-mode operation; internal callers are not an automatic CRUD/FLS exemption.

```apex
List<Contact> contacts = [
    SELECT Id, Email FROM Contact
    WHERE AccountId = :accId WITH USER_MODE
];
```

Consult [Secure Apex Classes](https://developer.salesforce.com/docs/platform/lwc/guide/apex-security) for current version-specific rules before changing access behavior.

## Step 4 — SOQL Injection Prevention

```apex
// ❌ NEVER — concatenates user input into SOQL string
String soql = 'SELECT Id FROM Account WHERE Name = \'' + userInput + '\'';

// ✅ ALWAYS — bind variable
List<Account> matches = [SELECT Id FROM Account WHERE Name = :userInput WITH USER_MODE];

// ✅ For dynamic SOQL with user-controlled field names — validate against a whitelist
Set<String> allowedFields = new Set<String>{'Name', 'Industry', 'AnnualRevenue'};
if (!allowedFields.contains(userInput)) {
    throw new IllegalArgumentException('Field not permitted: ' + userInput);
}
```

## Step 5 — Modern Apex Idioms

Use language features supported by the class API version. Prefer existing project style for cosmetic assertions; prioritize correctness and observable test outcomes:

| Old pattern | Modern replacement |
|---|---|
| `if (obj != null) { x = obj.Field__c; }` | `x = obj?.Field__c;` |
| `x = (y != null) ? y : defaultVal;` | `x = y ?? defaultVal;` |
| `System.assertEquals(expected, actual)` | `Assert.areEqual(expected, actual)` |
| `System.assert(condition)` | `Assert.isTrue(condition)` |
| Queries with an implicit operation access mode | `[SELECT ... WHERE ... WITH USER_MODE]` |

## Step 6 — PNB Test Coverage Checklist

Every feature must be tested across all three paths. Missing any one of these is a quality failure:

### Positive Path
- Expected input → expected output.
- Assert the exact field values, record counts, or return values — not just that no exception was thrown.

### Negative Path
- Invalid input, null values, empty collections, and error conditions.
- Assert that exceptions are thrown with the correct type and message.
- Assert that no records were mutated when the operation should have failed cleanly.

### Bulk Path
- Exercise representative bulk boundaries, including a full trigger batch where applicable; choose additional batch sizes for the actual entry point.
- Assert that all records processed correctly — no partial failures from governor limits.
- Use `Test.startTest()` / `Test.stopTest()` to isolate governor limit counters for async work.

### Test class checks

- Keep tests isolated from existing org data unless the platform-specific case requires and documents otherwise.
- Create synthetic records matching the actual schema and assertions; use the project's factory if present.
- Test empty input, denied access, expected failure behavior, and bulk records as applicable.
- Use meaningful assertions after asynchronous completion, not coverage-only execution.
- Treat code fragments above as illustrative: referenced variables come from the caller; compilation requires the project's actual class and schema context.

## Step 7 — Trigger Architecture Checklist

- [ ] Prefer a coherent entry point per object; flag fragmented triggers and propose a scoped migration rather than consolidating unrelated code automatically.
- [ ] Trigger body contains only: context checks, handler invocation, and routing logic.
- [ ] No business logic, SOQL, or DML directly in the trigger body.
- [ ] If a trigger framework (Trigger Actions Framework, ff-apex-common, custom base class) is already in use — extend it. Do not create a parallel pattern.
- [ ] Handler class is `with sharing` unless the trigger requires elevated access.

## Quick Reference — Hardcoded Anti-Patterns Summary

| Pattern | Action |
|---|---|
| SOQL inside `for` loop | Refactor: query before the loop, operate on collections |
| DML inside `for` loop | Refactor: collect mutations, DML once after the loop |
| Class missing sharing declaration | Review entry point/API version and recommend explicit intent |
| `escape="false"` on user data (VF) | Remove — auto-escaping enforces XSS prevention |
| Empty `catch` block | Add logging and appropriate re-throw or error handling |
| String-concatenated SOQL with user input | Replace with bind variable or whitelist validation |
| Test with no assertion | Add a meaningful `Assert.*` call |
| Assertion style differs from preference | Preserve working project conventions; prioritize meaningful assertions |
| Hardcoded record ID (`'001...'`) | Replace with queried or inserted test record ID |
