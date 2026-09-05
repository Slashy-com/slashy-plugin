---
name: setup-slashy
description: Connect and verify a Slashy account after installing the Slashy plugin. Use when Slashy tools are unavailable, disconnected, or need reauthentication.
---

# Set up Slashy

Slashy connects to the user's Gmail and Google Calendar through OAuth. Never ask the user to paste an access token, Google password, recovery code, or other secret into the conversation.

1. Check whether the `slashy` MCP server is connected.
2. If authorization is required, direct the user to the host's MCP or connector interface and let the host open Slashy's OAuth flow.
3. Ask the user to review the requested access and sign in to the Slashy account they intend to use.
4. After authorization, call `get_user_info` and report the connected inboxes, calendars, and timezone.
5. Do not send email, create events, or modify data merely to test the connection. Use a read-only search or calendar listing if further verification is needed.

If reauthentication fails, point the user to https://help.slashy.com/features/integrations/mcp and explain the observed error without repeatedly retrying.
