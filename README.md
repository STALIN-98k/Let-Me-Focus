# Let Me Focus

An offline-first focus companion: Pomodoro timer (with a progress border that
traces the perimeter of your screen), a to-do list that syncs straight to your
calendar, streaks, and daily check-ins. No account, no server — everything is
stored on your device.

## Install (same method as before)

1. Host these files on GitHub Pages: new **public** repo → upload all files to
   the repo root (not a subfolder) → Settings → Pages → Branch `main`, folder
   `/ (root)` → Save. Wait ~1 minute for the live URL.
2. Open that URL in Chrome on your phone → **⋮ → Add to Home screen**.

## Turning it into an APK (built for this)

The manifest is set up for maximum compatibility with PWA-to-APK tools:
- `"display": "fullscreen"` with a `display_override` fallback list, so there's
  no browser chrome — it opens exactly like a native app.
- A stable `"id"` field and both icon sizes with `maskable` purpose.
- A service worker that caches every asset, so the app works fully offline
  once installed — no network calls anywhere in this app.

To build the `.apk`:
1. Host the files (step 1 above) so you have a URL.
2. Go to **https://www.pwabuilder.com**, paste the URL, click Start.
3. Choose **Android** → Generate Package. PWABuilder creates the Trusted Web
   Activity wrapper and the `assetlinks.json` verification for you — that's
   what removes any browser UI when the APK is installed.
4. Download and sideload the `.apk`, or use the generated project in Android
   Studio directly if you want to customize further.

## What's in this build

- **Home**: greeting, context-aware motivational line, days-to-goal + streak,
  a full **Pomodoro timer** (Start/Pause/Skip/Reset, session counts), today's
  mission (today's + overdue to-dos), and a "Daily Check In" card that opens
  the full Check In tab. Copyright line sits at the bottom of this page only.
- **Pomodoro**: while running, a glowing progress border traces the entire
  screen perimeter (using an SVG rect with `pathLength`, so it's fully
  responsive to any screen size) — blue/purple during focus, green during
  breaks. Timing is timestamp-based, so it stays accurate even if you switch
  tabs or the app is backgrounded.
- **Calendar**: Day / Week / Month / Year views, any future year, custom
  events (exams, reminders, birthdays with recurrence) — and every to-do you
  add automatically appears on its due date here too.
- **To Do**: Overdue / Today / Upcoming / Completed, add via the + button,
  swipe-free delete button, all synced with the Calendar.
- **Check In**: overall completion ring, pace badge (ahead/behind/on track),
  predicted finish date, streak, tasks done this week, Pomodoro session counts.
- **Settings**: profile editor ("Save Mission" button), Pomodoro duration
  settings, reset data, About/credit section.

## Deliberately removed per your last round of changes

- The old Progress tab and Syllabus tab (replaced by Check In and To Do).
- The "weakest area" subject-detection feature (it depended on the old
  subject/topic model, which no longer exists).
- The Settings roadmap/notifications section.

## Data model (if you want to edit anything directly)

Look for the `db` object near the top of the `<script>` block in `index.html`:
`db.todos`, `db.events`, `db.pomodoro`, `db.profile` are the core structures.
Everything persists via `localStorage`, all on-device.
