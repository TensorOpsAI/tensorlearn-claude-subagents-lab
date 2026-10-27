# Role and artifact contracts
Model and tool choices are made when a role is run, not in the role files. Student JSON accepts `purpose`, `instructions` and `requestedContext` only. Roles are exactly researcher, implementer and verifier. Do not add model or tool fields.

Handoff: `{requirements:[{id,statement,source}],unresolved:[{question,owner}],sources:[{path,section}]}`. Do not resolve an open product decision without approval.
Implementation: `buildNotification(result)` in src/notification.mjs returns `{eligible,body}`. An unreleased or unreviewed result is ineligible with empty body. A released, reviewed result uses a neutral results-ready message without clinical data. `scheduleBatches(items)` returns ordered batches respecting the provider's current documented maximum batch size. The implementation role learns that limit through the handoff.
Findings: `{findings:[{path,line,severity,requirement,evidence,recommendation}]}`. Report only reproducible violations; also evaluate a clean patch: your own `src/notification.mjs` change (`git diff main -- src/`) should produce no findings. Findings are not committed; put them in your PR description.
