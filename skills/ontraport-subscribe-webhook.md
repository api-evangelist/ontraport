---
name: ontraport-subscribe-webhook
description: >-
  Subscribe an endpoint to one of Ontraport's four webhook events through the API, and
  verify delivery afterwards using the Webhook Log. Use when building an event-driven
  integration instead of polling collections.
api: ontraport:rest-api
generated: '2026-08-13'
method: generated
source: >-
  https://api.ontraport.com/doc/#webhooks, https://api.ontraport.com/doc/#webhook-events,
  https://api.ontraport.com/doc/#webhook-log, asyncapi/ontraport-webhooks.yml
operations:
  - POST /Webhook/subscribe
  - POST /Webhook/unsubscribe
  - GET /Webhooks
  - GET /WebhookLogs
---

# Subscribe to an Ontraport webhook

Ontraport publishes four events and lets you manage subscriptions through the API rather
than only through the app. Events were exposed 2018-03-06; the delivery log was exposed
2023-05-31.

## The four events

| Event | Fires when | Target in parentheses |
| --- | --- | --- |
| `object_create` | A new contact or custom object is added | object type |
| `object_submits_form` | A contact or custom object fills out the form | form id |
| `sub_tag` | A contact or custom object is added to the tag | tag id |
| `unsub_tag` | A contact or custom object is removed from the tag | tag id |

There are no commerce, task or automation events. If you need those, poll.

## Steps

1. **Subscribe.** Form-encoded POST, with the event target in parentheses:

   ```
   POST https://api.ontraport.com/1/Webhook/subscribe
   Content-Type: application/x-www-form-urlencoded
   Api-Key: <key>
   Api-Appid: <app-id>

   url=https%3A%2F%2Fyour.example.com%2Fhook&event=object_submits_form(1)&data=%7B%22format%22%3A%22lightweight%22%7D
   ```

   `data` is a JSON string of options; the documented key is `format`.

2. **Handle the payload.** The body is JSON:

   ```json
   {
     "webhook_id": "1",
     "object_type_id": "0",
     "event": {"type": "object_submits_form", "form_id": "4"},
     "data": {"id": "14", "email": "...", "...": "..."}
   }
   ```

   `object_type_id` tells you which record type fired. `data` carries the record's field
   values at the time of the event.

3. **Return a 2xx quickly.** Ontraport stores the status your endpoint returned in
   `last_code` on the webhook object, and the last body sent in `last_payload`. That is the
   only delivery signal on the subscription record.

4. **Audit deliveries.** `GET /WebhookLogs` (with `/WebhookLogs/meta` and
   `/WebhookLogs/getInfo` for fields and counts). Retention is 10,000 entries max —
   successes for 2 days, failures for 7. Pull failures inside that window or lose them.

5. **Unsubscribe** with `POST /Webhook/unsubscribe` when the integration is torn down.
   Orphaned subscriptions keep firing at a dead URL.

## Security warning

**Ontraport publishes no signature, shared secret, timestamp tolerance or IP allowlist for
webhook payloads.** A receiver cannot cryptographically establish that a payload came from
Ontraport. Treat every payload as untrusted input:

- Never act on the payload body alone for anything consequential. Re-read the record by ID
  through the authenticated API before acting on it.
- Use an unguessable path segment on the subscription URL as a weak shared secret.
- Never place a webhook receiver on an endpoint that mutates state without re-verification.

## Related

The Rules engine has a separate PING URL action, Ontraport's other outbound HTTP mechanism.
Since 2019-07-11 it takes only a Webhook ID; the old `url`, `post_data` and `json_flag`
parameters are deprecated.

## Rules

- No retry policy is published. Assume at-most-once and reconcile against the API.
- No AsyncAPI or event schema exists — the payload shape above is transcribed from
  Ontraport's example, not from a contract.
