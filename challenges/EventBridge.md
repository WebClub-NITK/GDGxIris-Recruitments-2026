## Task ID: EventBridge

#### `Full Stack Web Development`, `Real-Time Sync`, `Webhooks & Queues`

Mentors: [Pari Tibrewal](https://github.com/pari1011) ([+91 8240188219](https://wa.me/918240188219)), [Devansh Sharma](https://github.com/DevanshSharma351) ([+91 8218371950](https://wa.me/918218371950))

Difficulty: `Hard`

### Description

Build a lightweight webhook listener, event processing pipeline, and live monitoring dashboard. The system must ingest incoming payloads, process multi-step workflows, persist failure states on the server, and allow users to manually retry failed tasks.

**Example:** Receive a payment or GitHub event via webhook, parse and validate the data, log the payload to a persistent database, display the status on a real-time dashboard, and provide a manual **"Retry"** option if an external service fails.

---

## Features to Implement

### 1. Webhook Ingestion Endpoint
- Expose an API endpoint to receive incoming `POST` JSON payloads.
- Check a header secret on the server before you accept the body. The secret stays in an environment variable, not in the frontend and not in git.
- Parse and log the payload into a server database immediately upon receipt.
- Assign a unique Event ID and status `Received` to every new payload.
- If the same sender delivery is posted again, update the existing row. Do not create a second event. Use an idempotency key from the sender, or a hash of the raw body if the sender does not send one.

### 2. Live Event Dashboard (Real-Time UI)
- Display incoming webhook events in real time without requiring a manual page refresh (using WebSockets or SSE).
- The public page shows event id, timestamp, status (`Received`, `Completed`, `Failed`), and attempt count.
- Payload details are visible only to a signed-in viewer. A shared password or a single admin login is enough.
- Maintain consistent list order and preserve state across browser reloads or server restarts.

### 3. Pipeline Processing & Failure Handling
- Run this pipeline for every accepted event: validate the JSON, persist the row, then call one downstream step (a dummy HTTP API is enough).
- Handle a failed downstream call without crashing the server.
- Persist status `Failed` and a failure reason. A successful downstream call sets status `Completed`.

### 4. Manual Retry Mechanism
- Provide an interactive **"Retry"** button on the live dashboard for a failed event. Only a signed-in viewer can press it.
- Triggering retry must re-run the pipeline using the stored original payload.
- A second click while a retry is already running does not start another run.
- Update the event status to `Completed` once the re-attempt succeeds, and increment the attempt count.
- A failed retry stays `Failed`, records the new failure reason, and still increments the attempt count.

### 5. Mandatory Live Deployment
- The entire web application (frontend + backend + database) **must be publicly deployed** (e.g., Vercel, Render, Railway, Fly.io, or Supabase).
- A reviewer with the public URL and the header secret should be able to trigger webhooks via cURL or Postman and see the new row on the live site. The GitHub repo stays private. The running app is public.
- Localhost links will **not** be accepted.

---

### Mandatory Requirements

1. **Authentication & API Keys** - Secure the webhook endpoint with a header secret or basic API key verification.
2. **Real-Time WebSockets/SSE** - Webhook updates and retry status changes must reflect instantly across all open browser sessions.
3. **End-to-End Public Deployment** - App and database must be live on a public URL.

---

### Useful Resources

- [Webhooks Overview (MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Webhook)
- [Server-Sent Events (SSE)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)
- [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [Express.js Webhooks Guide](https://expressjs.com/)
- [Supabase Realtime](https://supabase.com/docs/guides/realtime)