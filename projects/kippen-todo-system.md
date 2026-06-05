# Kippen TODO System — Terminal Task Management & Automation

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.2.7-092E20?logo=django&logoColor=white)
![Daphne](https://img.shields.io/badge/Daphne-ASGI-44B78B)
![Server-Sent Events](https://img.shields.io/badge/Realtime-SSE-FF6F00)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)
![Let's Encrypt](https://img.shields.io/badge/HTTPS-Let's%20Encrypt-003A70?logo=letsencrypt&logoColor=white)

A web platform for coordinating daily work at a logistics terminal. It replaces radio
calls, whiteboards, and printed container lists with a shared, real-time task board,
an approval workflow, built-in messaging, and an automated import pipeline that turns
incoming train/container schedules into ready-to-claim tasks.

**Repository:** https://github.com/nexzitar/Posten

---

## The Problem

Terminal and dispatch work is coordinated under time pressure and traditionally relies on:

- **Radio communication** to hand out and confirm tasks, which is easy to mishear and
  impossible to audit.
- **Paper container lists** printed from schedules, which go stale the moment they're printed.
- **Manual hand-offs** with no clear record of who did what, when, or whether it was checked.

This makes it hard to see live status, balance workload across roles, and keep an audit
trail of completed and approved work.

## The Solution

A Django application that gives the whole team one live view of the work, accessible from
phones, tablets, and desktops on the terminal network.

- **Shared real-time task board** — when anyone claims, completes, or approves a task, every
  connected screen updates instantly (no manual refresh, no radio confirmation needed).
- **Approval workflow** — tasks flow through `pending → in progress → pending approval →
  approved / rejected`, so coordinators sign off on work and there's a full audit trail.
- **Direct messaging** — built-in 1:1 chat with unread counts and read receipts replaces
  ad-hoc radio chatter for non-urgent coordination.
- **Role-based access** for real terminal roles (Jernbanekipp, Trekker, B2–B5, Coordinator),
  with a training module to track who is certified for which role.
- **Bilingual UI** (English / Norwegian) so the tool fits the actual workplace.

## Automation Highlight: "Innhenting" Import Pipeline

The feature most relevant to operations: an automated pipeline that removes the
paper/manual-entry step entirely.

1. A nightly job drops the day's schedule (Excel `.xlsx` or `.pdf`) into a watched directory.
2. A management command parses the file — normalizing headers, filtering to the relevant
   train rows, and skipping irrelevant terminals.
3. Each relevant container becomes a `TodoTask` (categorized as *innhenting*) with the
   arrival date as its due date, sorted by arrival time — ready for workers to claim.

The result: container pickup work shows up on the board automatically every morning,
instead of being printed, read out, and manually tracked.

## Architecture & Tech Stack

- **Backend:** Django 5.2.7 (Python), organized into focused apps — `todo`, `messaging`,
  `training`, `user_management`, `innhenting`, `core`, and `navbar`.
- **Real-time:** Server-Sent Events (SSE) served over the Daphne ASGI server. Mutations
  publish a timestamp to the cache; long-lived async streaming views push a `refresh`
  event to clients, which re-render in place. Falls back to short-interval polling if SSE
  is unavailable. (No WebSockets / Channels required.)
- **Data import:** `pandas` + `openpyxl` for Excel, `pdfplumber` + `PyPDF2` for PDF.
- **Database:** SQLite, persisted in production via a host-mounted volume so data survives
  container rebuilds.
- **Internationalization:** Django `i18n` with language-prefixed URLs and translated UI.
- **Deployment:** Fully containerized with Docker behind an Nginx reverse proxy with
  automatic HTTPS (Let's Encrypt). Runs as a dedicated non-root user; routine updates are a
  single `deploy-update.sh` (git pull → rebuild → migrate → collectstatic → compile translations).

## Impact

- Removes radio confirmation and paper lists from the daily task loop.
- Gives coordinators a live, auditable record of work and approvals.
- Automates the most repetitive step (turning schedules into tasks) end-to-end.

## Screenshots

> Add screenshots / short GIFs to `projects/images/` and reference them below. Suggested
> shots: the real-time task board, the approval workflow, the "Innhenting" filter view,
> and the messaging panel.

<!-- Uncomment and update paths once the images are added:
![Real-time task board](images/kippen-task-board.png)
![Approval workflow](images/kippen-approval.png)
![Innhenting auto-generated tasks](images/kippen-innhenting.png)
![Messaging](images/kippen-messaging.png)
-->

_Screenshots coming soon._
