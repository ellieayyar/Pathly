# Pathly

A UX/UI case study: a client portal and staff console for an immigration institute that handles **student visas for Canada and France**.

**Live demo:** https://ellieayyar.github.io/Pathly/ (interactive, fictional data)

## The problem
- Case information was scattered across spreadsheets, a task board and email.
- Clients could not see their status, so they phoned for updates.
- Wrong or outdated documents led to long chains of reminders and calls.

## The idea
- **Client portal (mobile-first):** one clear answer to "where am I, what do you need from me, what happens next?" A nine-step path, a "What we need from you" list, deadlines, a camera-first document upload and a case timeline.
- **Staff console (desktop-first):** a review queue where a specialist approves or rejects a document with a reason and a new deadline, plus a "Needs a call" list that appears only after reminders go unanswered.
- **Status system:** nine states (needed, received, in review, needs fixing, complete, expired, waiting on third party, locked, not applicable) shown with icon + label + colour, and a separate **Overdue** deadline marker.

## What is in this repository
| Path | What it is |
|---|---|
| `index.html` | Demo home |
| `screens/` | Client dashboard, document upload, case timeline, staff review queue |
| `design-system.html` | Components, states and usage |
| `tokens.css`, `components.css` | Design tokens and component styles (plain CSS, light theme only) |
| `icons.svg`, `art.svg` | Icon sprite and line illustrations |
| `COMPONENTS.md` | Component notes (purpose, states, accessibility) |

It is a static site: no build step. To run it locally, serve the folder with any static server, for example `python -m http.server 5173`.

## Accessibility
Built towards WCAG 2.2 AA: contrast checked for every text pair, status never shown by colour alone, visible focus, 44px touch targets on the client side, reflow down to 320px, text resizing and text-spacing overrides, skip links and landmarks. Automated checks (axe-core) report zero violations on all pages. Screen-reader and real-device testing are still to do.

## Notes
- All names, dates and documents are fictional. Uploads and review actions are simulated.
- The visual style was inspired by a soft dashboard reference; the layouts, status system and components are original work.
- Fonts are loaded from Google Fonts for this demo. A production build should self-host Host Grotesk.
- The logo mark is a placeholder.
