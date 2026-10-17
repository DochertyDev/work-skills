---
name: platform-apex-logs-debug
description: "Diagnose Salesforce Apex debug logs and stack traces for exceptions, governor limits, SOQL/DML repetition, CPU, heap, and callout failures. Use for runtime log evidence; exclude Apex source authoring, test execution, and Agentforce session traces."
license: Apache-2.0
metadata:
  category: salesforce-certinia
  source: https://github.com/forcedotcom/sf-skills/tree/625863fbdee9cf4647cc964ccf6ea99b71373a83/skills/platform-apex-logs-debug
  author: "Salesforce, Inc. and contributors"
  upstream-commit: "625863fbdee9cf4647cc964ccf6ea99b71373a83"
---

# Salesforce Apex log diagnosis

Local modification (2026-10-17): adapted from Salesforce's `platform-apex-logs-debug` for supplied-log diagnosis without plugin dependencies or automatic org access.

## Inputs and scope

Use an Apex debug log, approximate transaction/time window, failing action, and relevant source if available. A supplied redacted log is sufficient; no Salesforce CLI or authenticated org is required. If no log is available, explain what evidence is needed and offer an analysis plan. Never invent a transaction, org alias, or successful retrieval.

Own exception/stack-trace, governor-limit, SOQL/DML repetition, CPU/heap, and callout diagnosis from runtime evidence. Apex authoring/review belongs to `salesforce-apex-quality` when available; standalone query optimization and Agentforce session telemetry are outside this skill. Related skills are optional, never assumed installed.

## Workflow

1. Establish log identity, transaction type, timeframe, namespaces, and completeness. Check for truncation and missing logging categories; missing events do not prove that an operation did not occur. Analyze separate async transactions separately.
2. Find the entry point and first causal exception. Trace `EXCEPTION_THROWN`, `FATAL_ERROR`, and available stack/code-unit events to class, method, and source line. Distinguish a handled exception from the transaction's terminal failure.
3. Read limit snapshots such as `CUMULATIVE_LIMIT_USAGE` / `LIMIT_USAGE_FOR_NS`. Record observed usage and applicable maximum from the log or current platform documentation. Do not assume all async contexts or namespaces share one limit, or substitute a synthetic event format for actual output.
4. Group repeated `SOQL_EXECUTE_BEGIN` and `DML_BEGIN` by source location and call path. Repetition suggests unbulkified logic but is not proof of a loop; confirm against source before concluding. A SOQL for-loop over one query is different from issuing a query each iteration.
5. Inspect method, query, allocation, and callout timing where present. Distinguish elapsed duration from Apex CPU usage. A row count or missing WHERE clause alone does not establish query selectivity; request a separately authorized query plan when necessary.
6. Classify findings as Critical (observed runtime failure or hard limit), Warning (evidenced headroom/performance risk), or Info (bounded improvement or evidence gap). Recommend the smallest plausible correction and say what evidence would confirm or refute it.

Useful signals and recommendation directions:

| Signal | Investigation | Possible correction after confirmation |
| --- | --- | --- |
| Repeated query at one call site | Follow caller/loop and earlier limit usage | Collect keys, query once, use maps |
| Repeated per-record DML | Check batching and downstream automation | Collect mutations and bulk-write |
| CPU/heap pressure | Check context, allocations, complexity | Reduce repeated work or memory; evaluate async trade-offs |
| Fatal null or empty-result failure | Compare stack and queried fields/results | Correct assumptions and guard missing data |
| Callout failure or delay | Inspect redacted status/timing and retry path | Bound timeouts/retries and preserve idempotency |

## Output

For each finding report: what failed; where (log lines and source location); root cause or explicitly labeled hypothesis; severity; proposed correction; and a test or rerun that would verify it. Include missing/truncated evidence and distinguish analysis from actual reproduction. Do not assign production-readiness scores from pattern matches.

## Access and privacy

Prefer supplied local logs and minimize quoted excerpts. Logs can contain record values, identities, tokens, and endpoint secrets: redact before sharing, keep artifacts outside public version control, and never upload logs to external analyzers without authorization.

Org retrieval is optional and requires explicit authorization for the org and log scope. Verify the installed CLI command help before using it. Select a specific transaction rather than downloading all logs. Missing tools or authentication means continue offline or report the prerequisite; never install or authenticate automatically and never print credential-bearing org-display output.

Creating trace flags/debug levels, tailing when it creates a trace flag, running tests or anonymous Apex, changing code, deleting logs, seeding data, and deploying are separate operations requiring explicit authorization. Diagnosis does not authorize them. For insufficient logs, propose a bounded nonproduction capture plan and stop before mutation.

For exact event definitions and limits consult the current [Apex debug-log reference](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_debugging_debug_log.htm) for the relevant API version. No live org, plugin-hook, or benchmark result is implied by this instruction file.
