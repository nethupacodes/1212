# Healio 4.0 — Implementation and integration status

## Included and working in the standalone HTML
- Premium visual polish: forest/sage/mint/ivory design direction, smoother card/button interactions, page transitions, keyboard focus styles and reduced-motion support.
- Existing wellness modules: activity, water, sleep, health measurements, workouts, nutrition/meal logging, skincare journal, mood, habits/goals, reminders, profile, reports, and settings.
- Dashboard summary and charts, progress ring, recent activity and upcoming reminders.
- CSV export, JSON backup, print-to-PDF workflow, and wearable CSV/JSON import with confirmation.
- English/Sinhala core navigation support and existing theme/accent preferences.

## Requires configuration or additional implementation
- **Google login:** frontend now posts the Google credential to `/api/auth/google`; it will only proceed after server-side verification. Configure `GOOGLE_CLIENT_ID`, an authorized JavaScript origin in Google Cloud, and run/deploy the backend. The starter returns a verified profile but does not yet issue production sessions or persist accounts.
- **Secure accounts and cloud sync:** not production-ready. Add a database, stable user records, server-managed Secure/HttpOnly/SameSite session cookies, CSRF protection where relevant, account recovery, deletion, audit logging and appropriate retention controls. Do not treat browser localStorage as protected health storage.
- **Live AI assistant:** backend endpoint `/api/ai/wellness` is included. Add `OPENAI_API_KEY` on the server only. The existing HTML assistant UI still needs to be wired to this endpoint for real conversational replies; no API key should ever be placed in HTML/JavaScript.
- **Health Connect / Apple HealthKit:** planning only. Direct Health Connect access requires an Android app/integration with user-granted permissions. HealthKit access requires an Apple-platform app and entitlements/permissions. This website supports importing exported CSV/JSON records instead.
- **Full Sinhala translation:** core navigation has a dictionary, but not every paragraph, dialog, error, report and dynamically generated message is translated yet. A complete translation pass is still required.
- **PDF reports:** currently uses browser print → Save as PDF, not a server-generated PDF.
- **Browser notifications:** reminders are checked while the page is open; push notifications when the browser is closed are not configured.

## Local development
1. Install Node.js LTS.
2. In `backend/`, copy `.env.example` to `.env` and fill in the OAuth client ID, AI key (optional), and exact allowed origin.
3. Run `npm install`, then `npm start`.
4. Serve `index.html` on the origin listed in `ALLOWED_ORIGIN` (for example via a local development server); do not open it as `file://`.
5. Click Google sign-in and enter `http://localhost:8787` as the backend URL if prompted.
6. In Google Cloud Console, create an OAuth client of type **Web application** and add the website origin to **Authorized JavaScript origins**. Use the matching Client ID in both the browser prompt and backend `.env`.

## Security note
This is a development starter, not a certified medical application. Before real users or sensitive health data are supported, add database-backed accounts, server sessions, access controls, secure backups, data deletion/export workflows, abuse monitoring, rate limits, deployment HTTPS, privacy disclosures and a security review. Never place API secrets in the browser or commit `.env` to source control.
