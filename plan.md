# Guwahati Flood Watch — Plan

## Scope
A production-oriented, public-facing community flood-reporting site for Guwahati that replaces localStorage demo data with shared server-backed reports, durable image storage, real coordinates, time-based freshness, calculated hotspots, community flagging, and a protected municipality dashboard. Reports remain anonymous; administrators use Manus OAuth. The service is a community-reported situational aid—not an official emergency feed, authoritative flood warning, or emergency service. Seeded prototype reports must not appear as live public reports.

## Implementation approach
- Keep the supplied React + TypeScript + Vite frontend, Express/tRPC runtime, Drizzle/MySQL schema, Manus OAuth, and managed deployment pattern. Enable the explicitly requested managed server and database.
- Use the managed Google Maps browser API through the Manus proxy; store latitude/longitude for every report, use Guwahati as the initial center, support location permission, place/drag pins, road/area search, and selectable report markers. Do not expose server credentials.
- Replace hard-coded seed reports and localStorage as the source of truth with server-backed public procedures: list, create, still-flooded, cleared, flag, and hotspot queries. Refresh public data every 30 seconds; expire active visibility six hours after the latest valid confirmation and retain history.
- Use additive MySQL migrations for reports, report updates, and moderation flags. Group events within approximately 125 metres while preserving distinct street/road names; report event count, highest depth, active reports, latest report, cleared confirmations, and available historical trends.
- Accept no-login community submissions, but validate and rate-limit on the server, reject duplicates, persist only pseudonymous reporter tokens (never raw names or IPs), and make flags/moderation auditable. Clearly label community data unverified and show the safe-reporting warning.
- Compress/re-encode images in the browser to remove EXIF, enforce type/size limits, upload through a validated server route that obtains platform storage presigns server-side, and persist stable object paths plus thumbnails—not base64.
- Add shareable report links, safe offline drafts/retry states, accessible mobile-first controls, and a protected `/admin` route using the existing Manus OAuth role check. Provide status/area/severity/date filters, inspection queue, map, status history, hotspot summaries, and CSV/PDF exports.
- Configure the container build and health endpoint for server-backed publication. Do not invent an official weather/flood feed, SMS/WhatsApp provider, Sentry key, custom domain, or backup service; integrate these only with an approved source/account. Identify unavailable operational requirements before public launch.

## Design direction
Three possible styles:
- 0.07 — Civic monsoon field notebook: tactile, legible, urgent without panic.
- 0.04 — Northeast transit control room: dense, technical, high contrast.
- 0.02 — Rainy-day public utility: softer, friendlier, more neighborhood-oriented.

Chosen: **Civic monsoon field notebook**.

### Design Movement
A modern civic field instrument: editorial hierarchy, generous paper-like space, and small operational cues inspired by weather stations and hand-marked road maps.

### Core Principles
1. See the situation before reading the explanation.
2. Make the next action obvious on a wet phone screen.
3. Turn individual reports into shared memory.
4. Use color as a status language, not decoration.

### Color Philosophy
Deep ink navy carries trust and municipal seriousness. Warm paper keeps the interface human and readable in bright daylight. Flood cyan marks live water and active evidence. Coral is reserved for warnings such as vehicle danger. Moss signals cleared or verified states.

### Layout Paradigm
A split field view: the map occupies the visual center, while a narrow operational rail carries status, reporting, and hotspots. On small screens the rail becomes a bottom action dock and the map remains the first screen.

### Signature Elements
- A small “rain line” motif in the wordmark and section dividers.
- Map pins with a soft pulse ring for fresh reports.
- Compact evidence labels such as “22 min ago” and “4 repeats this season.”

### Interaction Philosophy
Tap targets are large and forgiving. A selected pin always exposes the next useful action: verify, clear, or report another. No login, no multi-step navigation, no hidden status changes.

### Animation
Use short 160–220ms transitions for panels and buttons. Fresh pins pulse once, then settle. The report drawer slides up on mobile and in from the right on desktop. Avoid decorative motion that competes with the map.

### Typography System
Use a system sans stack for dependable rendering and a high-contrast display face through weight and letter spacing rather than a remote font dependency. Headlines are tight and editorial; labels are uppercase with generous tracking; body copy stays 14–16px for field readability.

### Brand Essence
The shared live memory of Guwahati’s flooded roads; practical, local, accountable.

### Brand Voice
Direct, neighborly, evidence-led.
- “See water ahead? Drop a pin.”
- “The places that keep flooding deserve a record.”

### Wordmark & Logo
A compact droplet-pin mark cut by two horizontal rain lines, paired with the Guwahati Flood Watch wordmark.

### Signature Brand Color
Flood cyan `#18B9C6` — bright enough to read outdoors and ownable against the ink background.

## Project structure
- `client/src/pages/Home.tsx`: public report map, discovery, report flow, status updates, report flagging, and hotspots.
- `client/src/pages/ReportDetail.tsx`: shareable individual report and status actions.
- `client/src/pages/Admin.tsx`: OAuth-protected municipality moderation, status history, summaries, and exports.
- `client/src/components/Map.tsx`: managed Google Maps proxy, Guwahati coordinates, clustered markers, location search, and draggable pins.
- `client/src/lib/trpc.ts`, `client/src/lib/i18n.tsx`, `client/src/lib/photo.ts`, and `client/src/lib/reporter.ts`: typed API outputs, English/Assamese/Hindi UI, photo compression, and pseudonymous reporter tokens.
- `client/public/sw.js`: production-only cached shell that bypasses APIs and report photos.
- `client/src/_core/hooks/useAuth.ts`: existing Manus OAuth session and admin permission flow.
- `client/public/manus-routes.json`: synchronized `/`, `/reports/:id`, and `/admin` page routes.
- `drizzle/schema.ts` and `drizzle/`: durable report, update, flag, upload, claim, and migration definitions.
- `server/routers.ts`, `server/floodRouter.ts`, and `server/floodData.ts`: validated public procedures, freshness/hotspot calculations, and protected admin operations.
- `server/floodUploads.ts` and `server/storage.ts`: validated JPEG upload handling and stable object paths through platform storage.
- `plan.md`: implementation and existing civic-field design decisions.
- `TODO.md`: outcome-level upgrade status, preserving the existing prototype scope and open operational requirements.
