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
