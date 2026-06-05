# Logistics Automation Portfolio

I design and build tools that solve real operational problems in logistics environments
such as terminals and dispatch operations. Most of my projects started as attempts to
remove manual work, radio communication, and paper-based processes.

## Projects

### Kippen TODO System — Terminal Task Management & Automation

A Django web platform for coordinating daily work at a logistics terminal. It replaces
radio calls, whiteboards, and printed container lists with a shared, real-time task board.

- **Real-time task board** with an approval workflow (`pending → in progress → pending
  approval → approved`) — every screen updates instantly via Server-Sent Events.
- **Automated "Innhenting" pipeline** — nightly Excel/PDF schedules are parsed and turned
  into ready-to-claim container pickup tasks automatically.
- **Built-in messaging**, role-based access for terminal roles, a training/certification
  module, and a bilingual (EN/NO) interface.
- **Tech:** Django 5.2.7, Daphne ASGI + SSE, pandas/pdfplumber, Docker + Nginx + Let's Encrypt.

📄 [Read the case study](projects/kippen-todo-system.md) · 💻 [Source code](https://github.com/nexzitar/Posten)

### Trailer Manager — Depot Trailer Tracking with Plate OCR & Geofencing

A Flutter mobile app that keeps a live database of every trailer in the depot. Adding a
trailer is a single camera capture — the app reads the license plate with on-device OCR,
records the GPS position, and uses geofencing to detect which terminal it's in — replacing
a paper-based logging process that was always out of date.

- **One-capture entry** — a single photo creates a record with image, plate, GPS, and terminal.
- **Automatic license plate OCR** — on-device ML Kit text recognition auto-fills the trailer
  number (Norwegian, Swedish, and Dutch plate formats), with no typing and no cloud service.
- **GPS + geofencing** — exact coordinates per entry and automatic terminal detection
  (LSO / B1–B5 / OT), plus an interactive satellite map with status-colored markers.
- **Status, history & roles** — quick status changes with a full timestamped audit trail,
  role-based access (Guest/User/Admin), and SMS-based onboarding.
- **Tech:** Flutter + Riverpod (Clean Architecture), Google ML Kit OCR, geolocator + Google
  Maps, Node.js/Express + PostgreSQL backend, Shorebird OTA updates.

📄 [Read the case study](projects/trailer-manager.md) · 💻 [Source code](https://github.com/nexzitar/LagerKontroll)
