---
name: slashy-email
description: Use the Slashy MCP connector for Gmail or Outlook mail, Google or Outlook Calendar, and Slashy reminders and saved content. Apply when a user asks to search or act on their connected mail, meetings, or Slashy data.
---

# Slashy

Slashy connects to the user's live Gmail or Outlook mail and Google or Outlook
Calendar through the Slashy MCP server. Connect and authorize the server at
`https://slashy.ctrlcenter.ai/mcp` before using these tools. Tool availability
depends on the host and connection; use only tools actually exposed there.
Every tool here acts on real data owned by a real person: mail that
actually sends, invitations that actually
notify attendees, events that actually disappear. Treat it accordingly.

## The three rules that matter most

1. **Resolve identity before anything time- or identity-sensitive.** `get_user_info`
   returns the signed-in address and timezone. "Tomorrow at 9" is meaningless without
   the timezone, and "did they reply to me" is meaningless without the address. Call it
   once at the start of such a task, not repeatedly.
2. **Read before you write.** Never compose a reply, label a thread, or move an event
   from a search snippet alone. Snippets are truncated and routinely omit the part that
   changes the answer. `list_messages` finds candidates; `read_thread` is what you
   reason from.
3. **Sending and deleting need explicit user intent.** Default to `draft_email`. Reach
   for `send_email` only when the user clearly asked to send. The same applies to
   `delete_event` and to any trigger deletion. If the instruction is ambiguous, draft it
   and say so — an unsent draft is a small inconvenience, a wrong send is not recoverable.

## Choosing a tool

**Mail.** `list_messages` searches and filters the mailbox; `read_thread` returns a full
thread. Compose with `draft_email`, send with `send_email`, and undo a pending scheduled
send with `cancel_scheduled_email`. Triage with the specific mail action tool for
the requested change, such as `archive_email_thread`, `mark_email_thread_read`,
`set_thread_category`, or `mark_thread_for_followup`; `snooze_email` handles timed
resurfacing. `create_label` / `update_label` manage the label set itself. Threads and drafts come back
with an `open_in_slashy` link; when the user wants to see an email, give them that link,
not a provider webmail link.

**Calendar.** `list_calendars` first when the user has more than one and the target is
unclear. `list_calendar_events` scans a window, `read_calendar_event` opens one.
`create_event`, `update_event`, `delete_event` mutate. For anything involving "when are
they free", `get_available_times` is the tool — do not infer availability by listing
events and reasoning about the gaps, because it will miss working hours, declined
invitations, and all-day blocks.

**Meetings.** `prep_meeting` assembles context ahead of a meeting. `web_search`
and `scrape_url` reach outside the mailbox — use them when the answer is not in
the user's own data.

**Reminders and automations.** `list_reminders` and `cancel_reminder` manage existing
reminders. `list_email_triggers` and `list_calendar_triggers` show standing
automations. `delete_email_trigger` and `delete_calendar_trigger` remove one when
the user explicitly asks. List first so you act on the trigger the user meant.
If the connected host exposes reminder or automation creation tools, use them
only when the user requests that action.

**Memory and snippets.** `read_memories` recalls durable facts about the user;
`upsert_memory` records one worth keeping. `create_snippet` saves reusable text;
`update_snippet` changes an existing snippet. Prefer recalling over re-asking
something the user already told Slashy.

**Files.** `request_file_upload` then `upload_file` then `check_upload_status` for the
presigned path; `download_file` and `read_file` to retrieve. `get_gmail_attachment`
retrieves an attachment from Gmail; do not assume it works for Outlook messages.

## Ordering that avoids the common failures

Replying to a thread: `list_messages` to locate it, `read_thread` to read all of it,
then `draft_email`. Quoting the wrong message in a long thread is the usual error, and
reading the whole thread is what prevents it.

Scheduling with someone: `get_user_info` for the timezone, `get_available_times` for
real availability, then `create_event`. Propose times to the user before creating
anything unless they have already agreed to a slot.

Acting on "the email from Sarah": search, then confirm which thread you found before
mutating it. Two people share a first name more often than you would expect.

## Details

Read [conventions.md](references/conventions.md) for date and timezone handling,
recipient and label semantics, pagination behaviour, and error recovery.
