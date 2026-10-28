# Guwahati Flood Watch — TODO

## Live flood map and report discovery
- [x] Provide a mobile-first public view with no login or install requirement.
- [x] Show current Guwahati flood reports as map pins with clear location, depth, recency, and status indicators.
- [x] Make fresh reports visually stronger and allow older reports to fade so stale information is less prominent.

## No-login reporting flow
- [x] Let anyone start a report from the map and choose a location.
- [x] Provide exactly three water-depth choices: ankle, knee, and too deep for vehicles.
- [x] Allow an optional photo attachment with an in-browser preview.
- [x] Confirm a new report visibly and keep it in the active map state.

## Community status updates
- [x] Let people mark an active report as “still flooded.”
- [x] Let people mark an active report as “cleared.”
- [x] Reflect the updated status and recency in the selected-report panel and map pins.

## Repeat-flood hotspot report
- [x] Rank locations that flood most often.
- [x] Show repeat count, severity, recent status, and a municipality-oriented summary.
- [x] Make the repeat-spot data the primary civic value proposition, not a secondary dashboard detail.

## Durable follow-up
- [x] Replace browser persistence with managed database records and durable photo storage. **Evidence:** the additive MySQL migration is applied; reports use shared database rows and JPEG photos/thumbnails use platform object storage.
- [x] Add municipality export/download for the hotspot report. **Evidence:** the protected municipality dashboard generates non-identifying CSV and print-to-PDF summaries.

## Confirmed website delivery scope
- [x] Import and run the website from the supplied ZIP archive. **Evidence:** the React/Vite source was extracted and the homepage and route manifest return HTTP 200 from the managed preview.
- [x] Preserve the existing pages, content, interactions, and responsive behavior defined in the supplied code. **Evidence:** the archive's application source and styles were retained; TypeScript check, build, and supplied tests pass.
- [x] Resolve integration or build issues needed to deliver a working website preview. **Evidence:** dependencies installed from the lockfile; `pnpm check`, `pnpm test` (9 tests), and `pnpm build` pass.

## Public-use upgrade
- [x] **Shared flood reports and real map** — Add database-backed reports; integrate a real map; support real coordinate selection; implement server-side expiry; and refresh live reports every 30–60 seconds. Reports must fade by age: 0–90 minutes strong; 90 minutes–4 hours faded; 4–6 hours stale; after 6 hours hidden from the active map but retained historically. Keep a community-reported, not official emergency instructions, disclaimer. **Evidence:** MySQL migration is applied; Google Maps proxy loaded from Preview; public query refreshes every 30 seconds; server filters active reports at six hours, retains history, and map pins use 90-minute/four-hour fade thresholds.
- [x] **Safe reporting and durable photos** — Support “Use my location,” area/street search, tap-to-place and drag-adjustable pins, depth, optional photo and short note, confirmation, shareable report link, “I’m safe” reminder, and offline drafts/retries. Validate photo type and size, compress images, remove EXIF metadata, upload to object storage, store only stable URL/key and thumbnail, and never store base64. Include duplicate detection, rate limiting, report flagging, moderation, basic spam detection, and the warning not to enter floodwater. **Evidence:** anonymous tRPC submission and status/flag procedures, JPEG re-encoding and two storage variants, draft retry controls, and admin moderation are implemented; server checks location/depth/length and rate limits submissions.
- [x] **Hotspots and civic reporting** — Cluster reports within 100–150 metres while keeping nearby reports on different roads separate. Show event count, highest depth, active reports, last report time, cleared confirmations, and trend versus prior monsoon periods. Add a protected `/admin` dashboard with active reports, top repeat hotspots, date/area/severity filters, CSV/PDF export, map of recurring locations, “Needs inspection” queue, status history, and monthly/monsoon summaries. **Evidence:** database-backed road-aware 125-metre event clustering and seasonal summary procedures power the OAuth-protected admin routes, filters, inspection queue, history and CSV/print exports.
- [x] **Reliable and accessible public experience** — Keep a mobile-first responsive layout, large touch targets, high contrast, minimal animation, an offline-friendly cached shell, loading/empty/retry states, slow-network handling, accessible labels, keyboard navigation, and English, Assamese, and Hindi language support. Aim for a report flow completable in under 20 seconds. **Evidence:** responsive controls, production offline-shell service worker, retry/empty/loading states, keyboard-labelled controls and English/Assamese/Hindi dictionaries are in place; automated type checking and nine unit tests pass.
- [ ] **Production operations and optional integrations** — Prepare a real public deployment and domain; add error monitoring and backups; support rainfall/weather overlays, road-segment history, monsoon comparisons, municipality inspection workflow, SMS/WhatsApp notifications, and a public API only when their data sources, service providers, credentials, and operational requirements are available and approved. Do not present fabricated or community-submitted data as official. **Remaining before a public launch:** no custom domain, error-monitoring provider, or backup destination was supplied/configured; weather/SMS/public API integrations require an approved source and credentials. Container deployment is configured, but publication and public visibility are not enabled.
