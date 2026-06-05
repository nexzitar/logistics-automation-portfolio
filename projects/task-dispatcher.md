# Task Dispatcher — Fair SMS Task Dispatch for Coordinators

![Flutter](https://img.shields.io/badge/Flutter-3.38-02569B?logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-3.10-0175C2?logo=dart&logoColor=white)
![Riverpod](https://img.shields.io/badge/State-Riverpod-00D1B2)
![SQLite](https://img.shields.io/badge/Database-SQLite-003B57?logo=sqlite&logoColor=white)
![SMS](https://img.shields.io/badge/Dispatch-SMS-4CAF50)
![Shorebird](https://img.shields.io/badge/OTA-Shorebird-FF5C00)

A Flutter mobile app that helps coordinators fairly hand out tasks to helpers via SMS.
When more than one person is available, the app automatically picks who should get the
next job, applies a cooldown so nobody is overloaded, and adjusts wait times per worker
based on equipment or physical capability — replacing ad-hoc calls and mental round-robin.

**Repository:** https://github.com/nexzitar/TaskDispatcher

---

## The Problem

Coordinators often spend their shift telling helpers what to do next. With multiple people
on the floor, fairness becomes a real operational issue:

- **Manual rotation** — keeping track of who got the last task is easy to get wrong under pressure.
- **Uneven capacity** — some workers move faster or slower because of equipment, health, or experience, but a one-size cooldown treats everyone the same.
- **No audit trail** — radio calls and verbal hand-offs leave no record of who was assigned what, or when fallback was needed because everyone was busy.
- **Interrupt-driven workflow** — the coordinator stops what they're doing to decide and contact the next person for every single task.

## The Solution

An offline-first Flutter app that runs entirely on the coordinator's phone — no backend
required for day-to-day dispatch.

- **Worker roster** — add helpers with phone numbers, drag-to-reorder round-robin priority, and per-worker **cooldown weights** (e.g. 0.5× for fast, 2.0× for slower).
- **Shift management** — toggle who is on shift, track tasks sent per shift, and mark lunch breaks with extended cooldowns.
- **One-tap dispatch** — compose a task message, preview the next eligible worker, and send via SMS (Android opens the native composer; iOS uses the system Messages app).
- **Task history** — searchable audit log with fallback flags, cooldown stats, and per-worker analytics.
- **Configurable settings** — base cooldown, lunch duration, message templates, and optional OTA APK updates.

## Automation Highlight: Round-Robin + Weighted Cooldown Dispatch

The feature that removes the coordinator's mental load: **automatic fair selection with
personalized recovery time**.

1. Filter to workers currently **on shift**, sorted by queue position.
2. Start after the last assigned worker and walk the rotation.
3. Pick the first worker whose cooldown has expired (or who has no cooldown).
4. If **everyone is still busy**, fall back to whoever becomes available soonest and
   **stack** the new cooldown on top of the remaining wait.
5. Apply cooldown: `baseMinutes × worker.cooldownWeight`, then record the assignment
   and open SMS with the task text pre-filled.

The result: the coordinator types the task once and taps send — the app decides who is
next and enforces fair spacing automatically, even when capacities differ.

## Architecture & Tech Stack

- **Mobile app:** Flutter / Dart with **Clean Architecture** (domain → data → application
  → presentation), **Riverpod 3** for state/DI, Material 3 UI with four main screens.
- **Core algorithm:** Pure Dart use cases — `GetNextWorker`, `AssignTask`, `StartLunch`,
  `ToggleShift` — fully unit-tested, zero Flutter dependencies in the domain layer.
- **Persistence:** SQLite (`sqflite`) with workers, task history, and app settings tables;
  100% offline-first, all data stays on device.
- **SMS:** `url_launcher` on iOS; Android platform channel to native SMS intent with
  multipart message support.
- **Updates (optional):** Shorebird code push for Dart-only patches; static-file OTA
  server (nginx + `update.json` + signed APK) for full native releases.

## Impact

- Removes manual "who's next?" decisions from every task hand-off.
- Keeps workload fair while respecting real differences in worker speed or capability.
- Gives coordinators a searchable history of every assignment, including fallback cases.
- Works fully offline — no server dependency for dispatch operations.

## Screenshots

> Add screenshots / short GIFs to `projects/images/` and reference them below. Suggested
> shots: the Workers screen with shift toggles and cooldown weights, Send Task with next-worker
> preview, History with fallback filter, and Settings with cooldown configuration.

<!-- ![Workers screen](images/task-dispatcher-workers.png) -->

_Screenshots coming soon._
