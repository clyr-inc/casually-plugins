---
name: report
description: Read Casually campaign or project results, save an evidence-backed marketing report for a defined period, and identify the next action from actual measurements.
---

Identify the intended project/workflow and reporting period. Use `list_marketing_workflows`, `get_marketing_workflow` and the current MCP schemas to find saved work. Use `analyze_marketing_workflow` with explicit `from` and `to` timestamps when a period is requested; clarify a material ambiguity in dates or timezone. That tool saves a report and its history. For a read-only all-time project overview, use `get_project_performance` and explain its all-time scope.

Report the actual audience, queue count, sent emails, measured opens/clicks, LP views and form conversions available in the response. Preserve null rates when a denominator is missing. Distinguish queue acceptance, a recorded send and confirmed delivery. Do not attribute all project LP conversions to email, or infer revenue, commercial impact or causal lift from incomplete tracking.

Give a short interpretation supported by the measurements and limitations. Use `propose_marketing_actions` when the user asks for next actions; saved recommendations can inform the next campaign. Do not fabricate forecasts or spend on generation while reporting. Link to saved results and include the reporting period and current state.
