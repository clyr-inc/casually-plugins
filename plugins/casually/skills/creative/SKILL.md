---
name: creative
description: Generate and save Casually project hero images, ad banners, or platform image/video assets with a quoted credit budget, then attach saved assets to draft LPs.
---

Find the intended project and inspect saved creatives before generating another asset. Use Casually's platform tools so results, costs and history stay with the team's project.

For LP heroes and banners, use `estimate_creative_generation` with the brief, actual project, dimensions and variation count. Read `get_ai_credit_balance`. Generate only within the user's authorized budget using `generate_project_creatives`, the quote's `pricingVersion`, `maxCredits`, a unique `requestKey` and the same brief. If generation was not authorized, present the cost before spending.

For other image/video services, discover `list_ai_services`, select a configured provider/kind suitable for the request, call `estimate_ai_asset`, then `generate_ai_asset` with the quote's pricing version and the authorized budget. Discover the schemas before choosing provider identifiers or models. Ask for no provider API keys; Casually manages them.

Poll `get_creative_generation` or `get_ai_asset` for the saved generation ID. After disconnection, resume from that ID; do not generate again to check status. Keep the same key and inputs for retries. Keep the MCP connection open while waiting. Wait at least `pollAfterSeconds` (or until `nextPollAt`) before each status read; `null` means the job is terminal, so stop polling. If an older server has no interval hint, wait at least 15 seconds. For `RECONCILING`, wait at least 60 seconds. Respect `retryAfterSeconds` and HTTP `Retry-After`, increase the interval after transient failures, and send only one status read at a time. Authentication or permission errors require reconnecting or correcting access, not a new generation. Show the saved asset IDs and preview URLs; report failures and refunds as the server records them.

To place a completed asset, read `list_project_landing_pages`, choose the correct LP/block, and use `attach_creative_to_landing_page`. Preserve pending approval. Image generation and attachment do not publish an LP. Human review is required for pending creatives before launch. Never switch projects, silently raise the budget, or claim a queued asset is finished.
