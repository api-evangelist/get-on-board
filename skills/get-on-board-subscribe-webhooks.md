---
name: Subscribe to Get on Board webhooks
description: Discover available event types and register an HTTPS endpoint to receive Get on Board webhook events such as job.match.
api: openapi/get-on-board-openapi-original.yml
operations: [listWebhookEvents, createWebhookEndpoint, listWebhookEndpoints, retrieveWebhookEndpoint, updateWebhookEndpoint, deleteWebhookEndpoint]
auth: BearerAuth (professional JWT, Bearer)
---

# Subscribe to Get on Board webhooks

Webhook management uses a **professional JWT** (`Authorization: Bearer <jwt>`),
refreshed via `POST /api/v0/auth_tokens`.

## Steps

1. **List available event types** — `GET /api/v0/webhook_events`
   (`listWebhookEvents`). Returns event ids such as `job.match`
   ("Job matching your preferences").
2. **Register an endpoint** — `POST /api/v0/webhook_endpoints`
   (`createWebhookEndpoint`) with form fields `endpoint` (an HTTPS URL) and
   `events[]` (the event ids to subscribe to).
3. **List / inspect subscriptions** — `GET /api/v0/webhook_endpoints`
   (`listWebhookEndpoints`), `GET /api/v0/webhook_endpoints/{id}`
   (`retrieveWebhookEndpoint`).
4. **Update or remove** — `PUT /api/v0/webhook_endpoints/{id}`
   (`updateWebhookEndpoint`), `DELETE /api/v0/webhook_endpoints/{id}`
   (`deleteWebhookEndpoint`).

## Handling deliveries
- Webhook payloads are **starting points**: they identify a resource by
  `id`/`type`. Fetch the canonical resource first
  (e.g. `GET /api/v0/applications/{application_id}`), then `expand[]` or add
  `process_id` scope as needed — do not rely solely on the event body.
- Errors are `{ message, code }`.
