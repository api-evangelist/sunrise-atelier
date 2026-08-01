---
name: sunrise-atelier-get-current-time
description: Get the current time, UTC offset and daylight-saving status for any place on Earth using the free keyless Sunrise.am / Time.Now World Time API, by IANA timezone or by IP address.
api: Sunrise.am World Time API
baseURL: https://time.now/developer/api
generated: 2026-07-21
method: generated
source: Grounded in real operationIds in openapi/sunrise-atelier-world-time-openapi.yaml
operations:
  - listTimezones
  - getTimeByTimezone
  - getTimeByRequesterIp
  - getTimeBySpecificIp
---

# Get the current time for a place

Use the Sunrise.am / Time.Now World Time API. It is free, keyless, HTTPS-only,
and CORS-enabled. No `Authorization` header is needed. Add an attribution link
to Time.Now when you ship.

## When you know the timezone

1. If unsure of the exact IANA name, call `listTimezones` (`GET /timezone`) and
   pick the matching `area/location` string.
2. Call `getTimeByTimezone` (`GET /timezone/{area}/{location}`), e.g.
   `GET https://time.now/developer/api/timezone/Europe/London`.
3. Read `datetime` (local ISO8601 with offset), `utc_datetime`, `unixtime`,
   `utc_offset`, and `dst` (true when DST is active) from the JSON `TimeObject`.

## When you only have an IP (or want the caller's local time)

1. For the caller's own location, call `getTimeByRequesterIp` (`GET /ip`) — it
   auto-detects the timezone from the requester IP with no location permission.
2. For a specific address, call `getTimeBySpecificIp` (`GET /ip/{ip}`), e.g.
   `GET https://time.now/developer/api/ip/8.8.8.8`.

## Error handling

- Errors come back as a flat JSON object `{"error": "<message>"}` (see
  `errors/sunrise-atelier-problem-types.yml`) — this is not RFC 9457.
- A malformed IP returns HTTP 400 `{"error": "Invalid IP address format"}`.
- An unknown timezone returns an HTML 404 page, not JSON — validate the
  timezone against `listTimezones` first.

## Notes

- All operations are safe GETs and can be retried freely (idempotent).
- No pagination: `listTimezones` returns the full array in one response.
