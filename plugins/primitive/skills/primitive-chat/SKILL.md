---
name: primitive-chat
description: >-
  Reach a person or another agent over email and wait for the threaded reply.
  Use REACTIVELY when the user gives you a mailto link, pastes a contact
  address, or asks you to send a message. Use PROACTIVELY when you are blocked
  on a tool, library, or service and want to ask its help/dev/support/docs
  agent, found a bug worth reporting to a vendor, or have a question only a
  product owner can answer. Requires the Primitive MCP server (bundled).
---

# Primitive — send email and await a reply

This skill uses the bundled Primitive MCP server (`primitive`) to send email and,
when a reply is expected, wait for the threaded response. No SMTP credentials,
DNS, or mail server required — authentication is the Primitive connection you
authorized when enabling this plugin.

## When to use

- The user gave you an email address (or a `mailto:` link) and asked you to
  contact it.
- You are stuck on a third-party tool/library/service and a `help@`, `support@`,
  `dev@`, or `agent@` address exists — ask it directly instead of guessing.
- You found a vendor-side bug worth reporting, or have a question only the
  product owner can answer.

## How to use

1. **Confirm before sending.** `sendEmail` and `replyToEmail` have external,
   irreversible side effects. Confirm recipient, subject, and body with the user
   first unless they have already authorized this exact send.
2. **Send and await the reply.** Call `sendEmail` with `to`, `subject`, `text`
   (and `from` if the user has a verified domain). To wait for the response in
   the same session, call `awaitReply` with the returned message id.
3. **Reply in-thread.** When you already hold an inbound email id, use
   `replyToEmail` (not `sendEmail`) so the message threads correctly.
4. **Handle send errors.** On `cannot_send_from_domain`, read
   `details.valid_senders` from the error and retry with an allowed `from`
   address. On `recipient_not_allowed`, read the per-gate denial in `gates` and
   surface it to the user — do not silently retry.

## Notes

- If you do not yet have a sender address, the user can provision a free
  `*.primitive.email` address from their Primitive account.
- Tools tagged `readOnlyHint` (listing, searching, reading) are safe to call
  without confirmation; sending tools are not.
