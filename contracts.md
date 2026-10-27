# Role and artifact contracts
The instructor renders actual model IDs, tool lists and scoped MCP servers. Student JSON accepts `purpose`, `instructions` and `requestedContext` only. Roles are exactly researcher, implementer and verifier. Models/tools in candidate files do not grant capability.

Handoff: `{requirements:[{id,statement,source}],unresolved:[{question,owner}],sources:[{path,section}]}`. Do not resolve an open product decision without approval.
Implementation: `buildNotification(result)` in src/notification.mjs returns `{eligible,body}`. An unreleased or unreviewed result is ineligible with empty body. A released, reviewed result uses a neutral results-ready message without clinical data. `scheduleBatches(items)` returns ordered batches respecting the provider's current documented maximum batch size. The implementation role learns that limit through the handoff.
Findings: `{findings:[{path,line,severity,requirement,evidence,recommendation}]}`. Report only reproducible violations; also evaluate a clean patch.
