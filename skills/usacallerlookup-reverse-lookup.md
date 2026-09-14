---
name: usacallerlookup-reverse-lookup
description: >-
  Identify a US phone number using USACallerLookup — carrier and location from NANPA data
  plus FTC Do Not Call complaint history — over the free, no-auth REST API.
api: USACallerLookup Free Phone Lookup API
operations:
  - lookupNumber
  - getStats
---

# Reverse-lookup a US phone number

Base URL: `https://www.usacallerlookup.com/wp-json/ucl/v1` — no API key, no sign-up.

## Steps

1. Normalise the number to exactly 10 NANP digits (strip a leading `+1`, spaces, dashes and
   parentheses). Reject anything that is not 10 digits before calling.
2. `GET /number/{phone}` (operation `lookupNumber`). A 200 returns a `NumberProfile`:
   carrier/`location`, `complaints` counts, `community_reports` and an `attribution` block.
3. Read complaint figures as *records filed against the number*, not proven wrongdoing —
   caller ID is trivially spoofed, so the displayed number may not have placed the call.
   Always surface the date range shown with the figures.
4. Never use this data for any FCRA-regulated purpose (credit, employment, housing);
   USACallerLookup is not a consumer reporting agency.

## Errors & limits

- `400` — not a valid 10-digit NANP number. Fix the input, do not retry as-is.
- `404` — `{ code, message, data }` WordPress route error for malformed paths.
- `429` — you exceeded 60 requests/minute per IP. Honor `Retry-After`, cache responses
  (`Cache-Control` is sent), or download the bulk CC0 dataset for high volume.
