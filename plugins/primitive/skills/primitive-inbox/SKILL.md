---
name: primitive-inbox
description: >-
  Receive email — check what arrived, wait for a specific message, and read
  full threads. Use REACTIVELY when the user wants an address to receive
  replies, verification codes, alerts, or signups, or asks "did anything come
  in?" / "check the inbox" / "wait for the reply." Use PROACTIVELY when you
  sent something and need to watch for the response, or are building an
  email-driven workflow that triggers on receipt. Requires the Primitive MCP
  server (bundled).
---

# Primitive — receive and read email

This skill uses the bundled Primitive MCP server (`primitive`) to read inbound
email that arrived at a managed `*.primitive.email` address or a custom verified
domain. No SMTP or polling loop to build yourself.

## When to use

- The user needs an address to receive a reply, verification code, alert, or
  signup confirmation.
- The user asks "did anything come in?", "check the inbox", or "wait for the
  reply."
- You sent something with the `primitive-chat` skill and need to watch for the
  response.

## How to use

1. **Check readiness.** Call `getInboxStatus` to confirm the inbox is set up
   (domain verified, route deployed) before telling the user it can receive mail.
2. **List or search.** Use `listEmails` for the most recent inbound messages, or
   `searchEmails` with sender/subject filters to find a specific one.
3. **Read the full thread.** Call `getEmail` for one message's parsed content and
   delivery state, or `getConversation` with an inbound email id to pull the full
   back-and-forth to pass as context to a model.
4. **Wait for a specific message.** When you expect a reply that has not arrived
   yet, use `awaitReply` against the message id you are waiting on rather than
   polling `listEmails` in a loop.

## Notes

- All reading tools are `readOnlyHint` — safe to call without confirmation.
- If the user has no inbox yet, they can provision a free `*.primitive.email`
  address from their Primitive account.
