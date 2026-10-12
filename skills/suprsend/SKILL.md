---
name: suprsend
description: Work with a SuprSend account through the SuprSend MCP server - users and their channels and preferences, objects, tenants, subscriber lists, broadcasts, workflows, templates, events, delivery logs and analytics (SQL), and the SuprSend docs. Use when the user mentions SuprSend, notifications, workflows, templates, subscriber lists, notification preferences, or delivery data. Before you write a workflow, template content, or SQL, read the matching SuprSend skill from the server (docs_get_skill).
license: MIT
---

# SuprSend

The SuprSend MCP server (`https://mcp.suprsend.com/mcp`) gives you live tools for the user's SuprSend account. The tools are named `<group>_<action>`, for example `users_get` or `workflows_save_draft`. The user signs in with their SuprSend account the first time that you call a tool.

## Read the SuprSend skill first

The detailed SuprSend skills are on the server. Read the matching skill before the work that it covers, and follow it:

- If your client lists SuprSend skills from the server (the MCP Skills extension, `skill://` resources), use them directly.
- Otherwise, call `docs_get_skill`. It returns the same files.

| Before you...                                                                                   | Read the skill                       |
| ----------------------------------------------------------------------------------------------- | ------------------------------------ |
| write or change workflow JSON (`workflows_save_draft`, `workflows_validate`)                     | `suprsend-workflow-schema`           |
| write template or variant content (`templates_upsert_variant`, `templates_validate`)             | `suprsend-template-schema`           |
| write SQL for `data_query`, `data_validate`, or a dynamic list query (`lists_preview_query`)     | `suprsend-semantic-layer`            |
| answer a how-to or concept question about SuprSend                                              | `suprsend-docs-support`, and `docs_search` |

With `docs_get_skill`, start without a `path`: you get the skill's `SKILL.md` and the list of its other files. Then read the files that you need with `path`, for example `{"skill": "suprsend-semantic-layer", "path": "references/sql-rules.md"}`.

## Rules

- **Workspaces.** An account has several workspaces (for example `staging` and `production`). When a tool needs a `workspace`, get the valid values from `workspaces_list`. Use `production` only when the user asks for it.
- **IDs.** Find a user's `distinct_id` from an email or a phone number with `users_find`. Find lists with `lists_list`, and other records with `data_query`.
- **Writes.** Tools that change data or send notifications ask the user first. Before a change that removes or replaces data (for example `lists_finish_replace` or `users_update`), say what changes.
- **Sending.** Call `workflows_trigger`, `broadcasts_trigger`, `workflows_test_run`, `templates_send_test`, or `events_track` only when the user asks to send. These send real notifications.
- **Broadcasts.** To send one published template to every user of a list, use `broadcasts_trigger`. Run it with `dry_run` first and tell the user the list size. Read the status with `broadcasts_get`, and stop a broadcast with `broadcasts_cancel` (both take the idempotency key that `broadcasts_trigger` returns).
- **Data limits.** `data_query` allows 10 queries per minute for the whole organization. Combine questions into one query when you can.
