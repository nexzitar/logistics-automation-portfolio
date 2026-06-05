# Trailer Manager — Depot Trailer Tracking with Plate OCR & Geofencing

![Flutter](https://img.shields.io/badge/Flutter-3.10-02569B?logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-3.10-0175C2?logo=dart&logoColor=white)
![Riverpod](https://img.shields.io/badge/State-Riverpod-00D1B2)
![ML Kit](https://img.shields.io/badge/On--device-ML%20Kit%20OCR-4285F4?logo=google&logoColor=white)
![Google Maps](https://img.shields.io/badge/Google%20Maps-4285F4?logo=googlemaps&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Shorebird](https://img.shields.io/badge/OTA-Shorebird-FF5C00)

A mobile app that keeps a live database of every trailer in the depot. Adding a trailer
is a single camera capture: the app reads the license plate from the photo with on-device
OCR, captures the GPS position, and uses geofencing to figure out which terminal the
trailer is standing in — replacing a paper-based logging process that was always out of date.

**Repository:** private (available on request)

---

## The Problem

Keeping track of which trailers are in the depot, where they are, and what state they're in
was done on paper:

- **Paper lists / whiteboards** that were stale the moment they were written and impossible
  to search.
- **No location data** — finding a specific trailer meant physically walking the terminal
  and reading plates by eye.
- **Manual data entry** of long license plate numbers, which is slow and error-prone.
- **No history** — there was no record of when a trailer arrived, changed status, or moved.

The result was wasted time looking for trailers, no shared real-time view, and no audit trail.

## The Solution

A Flutter app (currently on Android) backed by a Node.js/PostgreSQL API that gives the whole
team one live, searchable view of every trailer in the depot.

- **One-capture entry** — open the built-in camera, take one photo, and the record is created
  with photo, plate, GPS location, and detected terminal.
- **Automatic license plate OCR** — on-device text recognition reads the plate straight from
  the photo and auto-fills the trailer number, handling Norwegian, Swedish (classic and new
  `ABC 12D` format), and Dutch plate layouts.
- **GPS + geofencing** — every entry captures exact coordinates, and terminal geofences
  auto-detect which terminal (LSO / B1–B5 / OT) the trailer is in, so users don't have to
  pick it manually.
- **Interactive map** — all trailers on a satellite map with color-coded markers
  (empty / loaded / in ramp), filterable by terminal.
- **Status & history** — quick status changes (Empty, Loaded, In Ramp with ramp number),
  with a full timestamped history of who changed what.
- **Role-based access** (Guest / User / Admin) with SMS-based onboarding for new users.

## Automation Highlight: Camera → Plate → Location, in One Tap

The feature that removes the manual step entirely: turning a single photo into a complete,
located record.

1. The user opens the in-app camera (with flash/torch control) and captures the trailer.
2. Google ML Kit text recognition runs **on-device** on the photo, extracts candidate text,
   and matches it against known plate formats to auto-fill the trailer number — no typing,
   no third-party OCR service, works offline.
3. In parallel, the device captures the GPS fix and evaluates it against terminal geofences
   to pre-select the correct terminal.
4. The entry is saved to the backend with photo, plate, coordinates, terminal, and status.

The result: a trailer is logged in seconds with accurate data, instead of being hand-written
onto a list that nobody can search.

## Architecture & Tech Stack

- **Mobile app:** Flutter / Dart following **Clean Architecture** (data / domain /
  presentation), feature-first folders, **Riverpod** for state management and `go_router`
  for navigation.
- **OCR:** `google_mlkit_text_recognition` for fully on-device license plate reading.
- **Location & maps:** `geolocator` for GPS, `google_maps_flutter` for the satellite map,
  and custom geofence logic for automatic terminal detection.
- **Networking & storage:** `dio` HTTP client with JWT auth and retry logic, `hive` for fast
  local storage, and `dartz` for explicit `Either`-based error handling.
- **Backend:** Node.js + Express REST API, **PostgreSQL** (`pg`), `multer` + `sharp` for
  image upload/processing, `bcrypt` + `jsonwebtoken` for auth, `helmet` and rate limiting for
  hardening.
- **Updates:** **Shorebird** code push for over-the-air updates — bug fixes ship instantly
  without an app store submission.

## Impact

- Replaces paper lists with a live, searchable depot database.
- Cuts trailer logging to a single camera capture and removes manual plate typing errors.
- Gives the whole team a real-time map and status view, plus a full audit history.

## Screenshots

> Add screenshots / short GIFs to `projects/images/` and reference them below. Suggested
> shots: the camera capture with plate OCR, the satellite map with status markers, the
> trailer list with the terminal filter, and the trailer detail/history view.

<!-- Uncomment and update paths once the images are added:
![Camera capture with plate OCR](images/trailer-capture-ocr.png)
![Map with status markers](images/trailer-map.png)
![Trailer list and terminal filter](images/trailer-list.png)
![Trailer detail and history](images/trailer-detail.png)
-->

_Screenshots coming soon._
