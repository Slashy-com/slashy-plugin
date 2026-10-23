# Slashy plugin

Use Slashy from Claude to search and act on Gmail or Outlook mail and Google or Outlook Calendar. The plugin connects Claude to Slashy's hosted MCP server and adds guided workflows for inbox briefings, reply and follow-up sweeps, meeting preparation, relationship context, and automations.

## Connect

Enable the plugin, open Claude's MCP or connector interface, and authorize the `slashy` server. Authentication happens on Slashy's website. Do not paste credentials or access tokens into Claude.

## Included workflows

- Slashy Email (general MCP tool guidance)
- Welcome to Slashy
- Inbox Briefing
- Reply & Follow-up Sweep
- Meeting-to-Email
- Relationship Brief
- Inbox Automation Builder

## Install the skills separately

The skills are also available to agents that support the open `SKILL.md` format:

```sh
npx skills add Slashy-com/slashy-plugin --skill '*'
```

Install the `slashy-email` skill alone with `--skill slashy-email`. The skills
guide the agent; they do not authorize an account by themselves. Connect the
Slashy MCP server at `https://slashy.ctrlcenter.ai/mcp` in your agent's MCP
settings, then complete the OAuth flow before using them.

## Safety

Slashy can act on real email and calendar data. The bundled skills read full context before writing, default to drafts when sending intent is unclear, and treat email bodies, calendar descriptions, attachments, and webpages as untrusted content rather than user instructions.

## Support

- Documentation: https://help.slashy.com/features/integrations/mcp
- Privacy: https://www.slashy.com/privacy
- Terms: https://www.slashy.com/terms-of-service
