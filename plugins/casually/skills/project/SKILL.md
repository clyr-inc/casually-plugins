---
name: project
description: Prepare or update a shared Casually project, its service website and brand context, forms, draft LPs, history and review status.
---

Use discovered Casually MCP tools and keep assets attached to the intended shared project.

1. Call `list_creative_projects` before creating a project. Read the selected project with `get_creative_project`; discover existing brands with `list_brand_guidelines` and forms with `list_marketing_forms` when needed. Use real IDs returned by Casually.
2. Create with `create_creative_project` or update with `update_creative_project_context`. Mutations use a unique `requestKey`; updates use `baseUpdatedAt` from a fresh read. Reuse identical inputs and keys after uncertain outcomes. On `CONFLICT`, read current state and reconcile the user's requested changes without overwriting unrelated work.
3. For a user-supplied public service LP, read the latest project and call `register_project_service_site` with its `baseUpdatedAt`, `requestKey` and URL. It saves the actual site reference for campaign generation. Website registration and draft work do not establish domain ownership. Publication or site changes must pass Casually's ownership checks.
4. Reuse existing forms where suitable. `create_marketing_form` saves a draft definition; it does not submit a form or send a notification. For direct LP draft work, use `create_project_landing_page`, `get_project_landing_page` and `update_project_landing_page` with discovered renderer block types and real form/creative IDs. Preserve unrelated blocks and draft state.
5. Read `get_project_activity`, `get_project_review_status` or `get_project_performance` for the requested overview. Show project, edit and review links returned by the server.

Never infer ownership from a reference URL, bypass tenant checks, put provider credentials in context documents, or fabricate brand evidence. Publishing, bulk email and approval use the campaign workflow; project preparation does not imply authorization to launch.
