# QuickFix Mobiles Backend (Flask) — Routes

Base URL (local): `http://localhost:3001`

- Swagger/OpenAPI UI: `GET /docs`
- JSON OpenAPI spec: `GET /docs/openapi.json` (flask-smorest default)

## Environment Variables

See `.env.example` for the full list.

- `APP_ENV` (default `development`)
- `APP_PORT` (default `3001`)
- `ALLOWED_ORIGINS` (default `http://localhost:3000`) — comma-separated
- `SUPABASE_URL`, `SUPABASE_ANON_KEY` (optional; not required for these endpoints)
- `EMAIL_PROVIDER_API_KEY` (optional; if present we still return mock responses until real integration is implemented)
- `WHATSAPP_API_TOKEN` (optional; same as above)

---

## System

### `GET /`
Index route.

**Response 200**
```json
{
  "service": "QuickFix Mobiles Backend",
  "version": "v1"
}
```

### `GET /health`
Health check.

**Response 200**
```json
{ "status": "ok" }
```

---

## Bookings

### `GET /bookings`
List all bookings.

**Response 200**
```json
[
  {
    "id": "uuid",
    "service": "Screen Repair",
    "name": "John Doe",
    "phone": "+1 555 0100",
    "email": "john@example.com",
    "device_model": "iPhone 12",
    "preferred_date": "2026-02-01",
    "notes": "Please call before arriving",
    "created_at": "2026-01-01T00:00:00Z"
  }
]
```

### `POST /bookings`
Create a new booking.

**Request JSON**
```json
{
  "service": "Screen Repair",
  "name": "John Doe",
  "phone": "+1 555 0100",
  "email": "john@example.com",
  "device_model": "iPhone 12",
  "preferred_date": "2026-02-01",
  "notes": "Please call before arriving"
}
```

**Response 201**
Returns created booking object (adds `id`, `created_at`).

### `GET /bookings/{booking_id}`
Get booking by id.

**Response 200**
Booking object.

**Response 404**
```json
{
  "error": { "code": "not_found", "message": "Booking not found" }
}
```

---

## Contact

### `GET /contact`
List contact submissions.

### `POST /contact`
Submit the contact form.

**Request JSON**
```json
{
  "name": "Jane",
  "email": "jane@example.com",
  "phone": "+1 555 0200",
  "subject": "Repair quote",
  "message": "How much for a battery replacement?"
}
```

**Response 201**
Returns submission object (adds `id`, `created_at`).

---

## Articles

Articles are seeded so the API works immediately.

### `GET /articles`
List articles.

### `GET /articles/{article_id}`
Get article by id.

**Response 404**
```json
{
  "error": { "code": "not_found", "message": "Article not found" }
}
```

---

## Projects

Projects are seeded so the API works immediately.

### `GET /projects`
List projects.

### `GET /projects/{project_id}`
Get project by id.

**Response 404**
```json
{
  "error": { "code": "not_found", "message": "Project not found" }
}
```

---

## Notifications

### `GET /notifications`
List logged notification events.

### `POST /notifications`
Create/log a notification event (simulation).

**Request JSON**
```json
{
  "type": "custom_event",
  "payload": { "any": "json" }
}
```

**Response 201**
Returns created notification event.

---

## Integrations (Stubs)

These endpoints accept payloads, log them to server logs, and return mocked success responses.
They **do not** perform real external calls unless implemented later.

### `POST /integrations/email/send`
**Request JSON**
```json
{
  "to": "customer@example.com",
  "subject": "Booking confirmed",
  "message": "Thanks for booking with QuickFix Mobiles!"
}
```

**Response 200**
```json
{
  "success": true,
  "mode": "mock",
  "provider": "email",
  "message": "Email send accepted (mock)."
}
```

### `POST /integrations/whatsapp/send`
**Request JSON**
```json
{
  "to": "+15550100",
  "message": "Your device is ready for pickup."
}
```

**Response 200**
```json
{
  "success": true,
  "mode": "mock",
  "provider": "whatsapp",
  "message": "WhatsApp send accepted (mock)."
}
```
