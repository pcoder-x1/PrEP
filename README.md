# PrEP — PWA setup

```
prep-pwa/
├── index.html
├── manifest.json
├── service-worker.js
├── icons/            (11 sizes, including maskable variants)
└── screenshots/
    ├── wide.png       (used inside manifest.json for richer install UI)
    └── narrow.png
```

## Hosting (same as before)

Browsers only allow service workers and install prompts on **https://** or
**http://localhost** — not on a file opened directly from disk. To try it:

```
python3 -m http.server 8000
```
then open `http://localhost:8000`. To actually deploy it, drag the folder
onto Netlify/Vercel, or push it to a GitHub Pages repo — same steps as the
Pillbox app.

## Notes on the features you asked for

- **Today**: the two dose buttons are a toggle (only one selected at a
  time). Below "Taken at" there's now a **Date** field, defaulting to
  today and capped at today (`max` attribute plus a JS check, so a
  future date can't be typed in either). This date field is the *only*
  place doses get added or removed — pick a past date here to backfill
  or correct history, and the entry list right below switches to show
  that date's entries instead of always showing today's. **Only one entry
  per day is allowed** — the Add button disables itself once a date
  already has an entry, and there's a fallback alert if that's ever
  bypassed. Delete the existing entry first if you need to log a
  different one for that day.

- **History & upcoming calendar**: a day turns **light green** if it has
  a 1-pill entry, or **medium blue** if it has a 2-pill entry (a day with
  a 2-pill entry also gets a small **⯈** "start date" badge, applied
  automatically). Every day with an entry now also shows **1 or 2 small
  dots centered in the cell**, echoing the pill count at a glance without
  needing to click in. Appointment days show a **🧑‍⚕️** marker in the
  corner instead of a plain dot. Clicking a day opens a **read-only**
  detail panel — it shows that day's dose entry (with the same dot
  indicator) but has no add or delete controls; editing happens only
  through the Today section's date field. The old explanatory line about
  future dates has been removed from this panel — future days just show
  what's there (appointments) without extra text.

- **Doctor's appointment** (its own section, above Reminders): date +
  time — no flag checkbox, every appointment always shows the 🧑‍⚕️ marker
  on the calendar. The **"Add to calendar" / `.ics` export button has been
  removed** per your last request, along with the now-unused export code
  behind it.

- **Sexual activity section has been removed entirely** — no more
  dedicated section, status line, or button. What's left is just the
  🔥 flag itself: the calendar badge and legend entry still work exactly
  as before, and you can still log or un-log a date's activity via the
  checkbox in that day's read-only detail panel (click the day, then
  toggle "Activity" at the bottom). In its place, there's now a small
  note under **Reminders**: *"For users taking PrEP on demand, remember
  to continue taking your doses after the last sexual activity as per
  health care instructions."*

- **Reminders**: same permission flow, same 30-second check loop, same
  "Set a phone alarm" handoff, same "Daily at" time field as before. The
  **Enable reminders / Set a phone alarm buttons are now sized to match
  the time input's box** (same 41px height, bottom-aligned with it)
  instead of looking visually smaller next to it.

- **Backup**: Export tries `navigator.share()` first (the phone's share
  sheet — Files, Drive, email, AirDrop, etc.), and falls back to a plain
  download if the browser doesn't support sharing files (most desktop
  browsers). Import validates the file, shows a summary of what it
  contains, and asks for confirmation before replacing your current data.

- **Notices**: both notices are reproduced verbatim in a dedicated,
  smaller-text section at the bottom, exactly as written.

## A couple of honest caveats

- The iOS "Set a phone alarm" button is best-effort only — Apple doesn't
  publish a way for a website to open the Clock app to a specific screen,
  so on iPhone it may just show you where to go manually instead.
- `navigator.share()` with files needs a secure context (https) and a
  browser that supports the File Share API — it works on current mobile
  Safari and Chrome, but desktop browsers will usually fall back to a
  plain download instead of a share sheet.
- Since doctor's appointments no longer export to `.ics`, adding one to
  your phone's actual calendar now means doing that manually in your
  Calendar app using the date/time shown here.
