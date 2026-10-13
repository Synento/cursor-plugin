---
name: synento-video
description: Add live video calls with built-in recording to an app using Synento, and verify that camera and microphone media actually arrived. Use when the user asks for video calls, live video, call recording, or a Synento integration.
---

# Synento video integration

Synento is an agent-native video API: every live session is also its own
recording. The `synento` MCP server in this plugin lets you set up a project,
write the integration, and then prove it works by reading the recording back.

## When to use

- The user wants live video or audio calls in their app
- The user wants calls recorded, replayed, or exported
- The user asks to add, fix, or check a Synento integration

## Instructions

1. Call `get_account` to see the organisation, the projects this connection
   may use, and what is left to do. If there is no suitable project, call
   `create_project`.
2. Call `create_server_key` to mint a restricted `rk_live_` key for the
   project. Write it to the app's server-side environment (for example `.env`,
   which must be gitignored):

   ```bash
   SYNENTO_API_KEY=rk_live_…
   SYNENTO_API_URL=https://api.synento.com
   SYNENTO_PROJECT_ID=…
   ```

   Never put this key in browser code and never commit it.
3. Read `get_guide("getting-started")` and `get_guide("guides/sdk")`, then
   write the integration:
   - the app's server creates sessions and mints connection tokens with
     `@synento/node`;
   - the browser joins with `@synento/client`, using only the short-lived
     connection token.
4. Run the app and have it publish camera and microphone into a session.
5. Call `verify_session` on that session id. It reads the recording back and
   reports, per participant, which tracks actually carried media.

## Rules

- Do not tell the user the integration works until `verify_session` passes.
- If `verify_session` keeps failing, call `run_media_check`. It sends a
  synthetic call through Synento's own pipeline, which tells a Synento problem
  from an integration problem. It does not run the user's code, so it cannot
  prove the integration works.
- Common causes of a failed check: no audio track in the published stream,
  publishing before `connect()` resolved, or a connection token minted for a
  different session.

Full guide: https://synento.com/docs/guides/mcp
