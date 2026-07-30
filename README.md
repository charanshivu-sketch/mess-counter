# Mess Counter — setup notes

Two files matter: `index.html` (the app) and `roster.csv` (the name list). Keep them in the same folder.

## Put it on GitHub Pages

1. Create a repository — name it something like `mess-counter`. Public is fine; there is nothing confidential beyond names.
2. Upload `index.html` and `roster.csv` to the root of the repository.
3. Settings → Pages → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)` → Save.
4. Wait about a minute. The address will be `https://<your-username>.github.io/mess-counter/`.

## Two links, two roles

| Who | Link | Can do |
|---|---|---|
| Warden | `https://…/mess-counter/` | Count meals, add guests, send the day's file |
| You | `https://…/mess-counter/?admin=pltc2026` | Everything above, plus the ⚙ name list |

The ⚙ button is invisible without the `?admin=` code, so the warden has no way into the name list. Change `pltc2026` to something else on the `ADMIN_CODE` line near the top of the `index.html` script, and don't send him the admin link.

This hides the controls; it is not a password. Anyone who reads the page source could find the code. That is a reasonable trade for a mess register — say the word if you want a real login and we'll look at a backend.

**On his phone:** open the plain link in Chrome → ⋮ menu → **Add to Home screen**. It then opens full-screen like an app.

## Changing the name list

Edit `roster.csv` in GitHub (pencil icon → edit → Commit changes). The warden's app picks it up the next time he opens it with signal, and shows "Name list updated from the office".

Columns:

```
ID,Name,Group,Joined,Left
SEW1270,RAKESH KUMAR,worker,2026-08-12,
SEW1105,SAIFUL ISLAM,worker,,2026-08-20
```

- **Group** — `staff`, `worker`, or `hire`. Leave it blank and it's guessed from the ID prefix.
- **Joined** — first day they eat. Blank means from the beginning.
- **Left** — the first day they do **not** eat. Blank means still here.

Use **Left** rather than deleting the line. Deleting removes the person from days already counted, which means meals the caterer will still invoice for vanish from your record.

If you'd rather not hand-edit CSV: open the admin link, use the ⚙ screen to add, correct or mark people as left, then tap **Download roster.csv for GitHub** and upload the file it gives you.

## What the warden does each day

1. Open the app. The date defaults to today; the calendar button changes it to any date.
2. Pick the meal at the top.
3. Tap **Serve everyone**, then tap the few who didn't come. Or tap people one by one.
4. **Send day** → *Send today's file to the office*. On Android this opens the share sheet with the CSV attached, so he picks WhatsApp and sends. Where the phone doesn't support sharing files, the file goes to Downloads and WhatsApp opens with the summary text for him to attach it.

## Carry forward

Opening a day with nothing recorded copies the counts from the most recent day that has data, up to seven days back. An amber bar says which day it came from.

The copied numbers are a starting point, not a record. Until he taps something or sends the day, the CSV carries the remark *"Copied from the previous day — not yet checked"* in every row, and the monthly export keeps that remark. So if a day was never actually reviewed, you can see it in the data rather than paying for it.

To switch carry-forward off, change `cfg.carry` handling — or tell me and I'll add a toggle on the admin screen.

## Where the data lives

On the warden's phone, in the browser's storage for that web address. Nothing is on a server.

That means:

- Clearing browsing data, or "clear site storage", wipes the counts. The name list would come back from `roster.csv`; the counts would not.
- His phone is the only copy until he sends a file.

So the daily send is the backup, not just a report. If you want the data in one shared place automatically — two devices, or counts landing in a Google Sheet without anyone sending anything — that needs a small backend behind the app. A Google Apps Script web app writing into a Google Sheet is the usual free route, and the app is structured so it can be added without redesigning anything.
