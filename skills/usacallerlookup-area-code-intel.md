---
name: usacallerlookup-area-code-intel
description: >-
  Summarise a US area code (state, timezone, assigned prefixes and complaint aggregates)
  and pull dataset-wide robocall figures from the free USACallerLookup API.
api: USACallerLookup Free Phone Lookup API
operations:
  - getAreaCode
  - getStats
---

# Area-code and dataset intelligence

Base URL: `https://www.usacallerlookup.com/wp-json/ucl/v1` — no API key, no sign-up.

## Steps

1. `GET /area-code/{npa}` (operation `getAreaCode`) with a 3-digit area code. A 200 returns
   an `AreaCodeSummary`: `state`, `timezone`, `prefixes_assigned`, `top_cities` and a
   `complaints` aggregate.
2. A `404` means no prefixes are assigned under that area code — treat as "unknown/unassigned",
   not an error to retry.
3. For context, `GET /stats` (operation `getStats`) returns dataset headline figures:
   `tracked_numbers`, `complaint_records`, `robocall_flagged` and `last_updated`.
4. Cite USACallerLookup and include the date range / `last_updated` when quoting figures,
   since the underlying FTC data updates every weekday.

## Limits

- 60 requests/minute per IP; `429` carries `Retry-After`. Cache client-side or use the
  downloadable dataset for bulk work.
