# Guwahati Flood Watch — Public Launch Checklist

## Implemented in the project

- Shared MySQL-backed anonymous reports and status history; the additive schema migration has been applied.
- Real Guwahati map, location search and geolocation, map-pin placement/adjustment, 30-second live refresh, and six-hour active-map expiry with historical retention.
- Compressed JPEG photo upload to platform object storage, thumbnails, EXIF removal through browser re-encoding, upload limits, and duplicate/report-flag protections.
- Calculated road-aware hotspots and a protected municipality dashboard with review queues, filters, history, summaries, CSV export, and print-to-PDF.
- English, Assamese, and Hindi language controls; mobile layout; offline draft storage and an offline app shell.
- Prominent warnings: community reports are unverified; this is not an official warning or emergency service; never enter floodwater.
- **Verified in Preview:** the production Express entrypoint returns HTTP 200 for the health route, homepage, `/admin`, report-detail route, route manifest, service worker, and runtime config endpoint; read-only active-report/hotspot queries return HTTP 200; the Maps JavaScript proxy returns HTTP 200 for the Preview origin. `pnpm check`, all 9 tests, and `pnpm build` pass.

## Before announcing it publicly

1. **Build and publish:** use the Manus website Dashboard to set the project visibility to Public and publish the saved version. Automatic publication is off. A custom domain was not supplied; attach and verify DNS/TLS before printing a domain on materials.
2. **Claim the municipality administrator:** open `/admin` on the published site, sign in with the intended Manus account, and use the one-time setup token supplied securely for this project. After the first administrator is claimed, remove the bootstrap token from project secrets.
3. **Verify the map on the published origin:** check that the managed Maps API, billing/quotas, geocoding and browser key restrictions allow the final domain. Search and device-location selection remain available if map rendering fails, but pin placement and the public map should be confirmed before launch.
4. **Set operations:** configure an error-monitoring provider and scheduled database backups/restore practice. These require an approved provider and are not configured in this project.
5. **Scale abuse controls:** the current IP limiter is privacy-preserving but process-local; it resets on server restart and is not shared across multiple replicas. For sustained public traffic, add a distributed rate limiter or gateway protection before relying on the limits as a hard abuse barrier.
6. **Establish the official-data boundary:** this release displays community reports only. Add an official weather/flood feed only after an agency, source, licence, update cadence, alert owner, and fallback procedure are approved. Keep community and official information clearly separated.
7. **Launch smoke check:** test anonymous report creation, confirmation, clearing, duplicate rejection, flagging, moderation, CSV/PDF output, photo loading, keyboard/mobile use, and offline draft retry on the final public URL. Start with a small pilot and a named moderator on duty.

## Safety and privacy

Do not present community submissions as verified or direct people into floodwater. Provide the applicable privacy notice, contact/escalation procedure, retention policy, and local emergency instructions before a public campaign. The reporting service is a situational aid, not an emergency-response channel.
