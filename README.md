# CareCircle — Remote Eldercare Coordination (Python)

A web app that lets families in different cities coordinate appointments, medications, home-care visits, transport and emergency contacts for an older relative — in one shared place, with **the older person in control of what is shared**.

## The Problem
Adult children living away struggle to coordinate care. Information is scattered across calls and messaging apps. Families need trusted visibility without compromising an older person's independence or privacy.

## Who It Is For
- **Primary users:** older adults, adult children living away, family caregivers
- **Buyers/partners:** families (monthly subscription), employers (eldercare benefit), senior-living operators, home-care agencies (service commissions)

## Core Features (MVP)
1. **Care Circles** – one circle per older adult; invite family, neighbours, professional carers.
2. **Consent & Privacy Controls** – the elder (or their delegate) sets per-member, per-category visibility (appointments, medications, health notes, location, finances = never stored).
3. **Shared Calendar** – appointments, home-care visits, transport; assign "who is taking them".
4. **Medication Routines** – schedules with daily "taken / skipped" check-ins and missed-dose alerts to chosen members only.
5. **Task & Transport Board** – claim tasks (pharmacy pickup, ride to clinic) to avoid duplicated effort.
6. **Emergency Card** – contacts, conditions, allergies, doctor, preferred hospital; printable and shareable via secure link.
7. **Activity Feed & Audit Log** – who saw or changed what.
8. **Provider Directory (stub)** – home-care agencies and drivers, with a booking-request flow (basis for commission model).
9. **Org Portal (stub)** – employer/agency dashboard with aggregate, non-identifying usage stats.

## Tech Stack
- Python 3.11+, **FastAPI**, Jinja2 templates, **HTMX** + Tailwind (CDN) for UI
- SQLAlchemy 2.x + SQLite (swap to PostgreSQL for production)
- Auth: session cookies, `passlib` (bcrypt) password hashing, CSRF protection
- Background jobs: APScheduler (reminders, missed-dose checks)
- Tests: pytest

## Getting Started
```bash
git clone <your-repo-url>
cd carecircle
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python -m app.seed               # loads demo data
uvicorn app.main:app --reload
```
Open http://127.0.0.1:8000

Demo logins (seed data): `maya@example.com` (daughter, admin), `arun@example.com` (son), `lakshmi@example.com` (elder) — password `demo1234`.

## Project Structure
```
carecircle/
├── app/
│   ├── main.py            # FastAPI app entry
│   ├── config.py
│   ├── database.py
│   ├── models.py          # User, Circle, Membership, Consent, Appointment, Medication, ...
│   ├── schemas.py
│   ├── auth.py
│   ├── permissions.py     # consent-aware access checks
│   ├── routers/           # circles, calendar, medications, tasks, emergency, providers, org
│   ├── services/          # reminders, notifications (email stub), audit
│   ├── templates/
│   ├── static/
│   └── seed.py
├── tests/
├── requirements.txt
├── .gitignore
└── README.md
```

## Privacy Principles
- Elder-first consent: nothing is visible to a member unless the elder grants it.
- Data minimisation: no financial credentials, no continuous location tracking.
- Every view/edit of sensitive data is logged and visible to the elder.
- Exportable and deletable data per circle.

## Business Model Hypotheses (to validate)
| Segment | Model | Validation question |
|---|---|---|
| Families | Monthly subscription | Will the paying child adopt it if the parent barely uses a smartphone? |
| Employers | Per-employee benefit contract | Does it reduce caregiver absenteeism enough to justify cost? |
| Senior living / agencies | SaaS + family portal | Do they want a family-facing visibility layer? |
| Providers | Commission on bookings | Will families book through the app vs. calling directly? |

## Roadmap
- [ ] WhatsApp/SMS reminders for low-tech elders
- [ ] Voice-based check-ins
- [ ] Multi-language (Hindi, Assamese, Bengali, English)
- [ ] Payment integration (subscription + provider bookings)
- [ ] Mobile PWA

## License
MIT
