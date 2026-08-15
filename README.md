# Dentist-Appointment-Tracker
A calendar and clinical logbook that runs on an iPhone home screen.

# Clinic Log — Setup and Handover

A calendar and clinical logbook that runs on an iPhone home screen. Free to build, free to run, no App Store, no subscription.

This document covers putting it online, installing it on her phone, how each part works, and what can go wrong.

---

## 1. What you have

One file: `clinic-calendar.html`. Everything is inside it — layout, styling, logic. No build step, no npm, no dependencies to install. You can open it in any browser right now and it works.

Three sections:

| Section | What it does |
|---|---|
| **Calendar** | Month view, appointments per day |
| **Caseload** | Flat list split into Pending and Completed |
| **Quota** | Clinical logbook — 4 departments, 23 procedures, 89 cases |

Plus a **gear button** for backup and restore.

---

## 2. Putting it online

You need a public URL before it can go on her phone. Both accounts below are free and neither asks for a card.

### Step 1 — GitHub

1. Go to `github.com` and make an account.
2. Click **+** (top right) → **New repository**.
3. Name it anything, e.g. `clinic-log`. Leave it **Public**. Click **Create repository**.
4. Click **uploading an existing file**.
5. Drag in `clinic-calendar.html`.
6. **Rename it to `index.html`.** This matters — `index.html` is the name a web server looks for by default. Without it the site shows a file listing instead of the app.
7. Click **Commit changes**.

### Step 2 — Turn on GitHub Pages

No second account needed.

1. In your repository, click **Settings**.
2. **Pages** in the left sidebar.
3. Source: **Deploy from a branch**. Branch: `main`, folder: `/ (root)`.
4. **Save**.

After a minute you get a URL like `yourusername.github.io/clinic-log/`. Open it — the app should load.

The repository must stay **Public** for free GitHub Pages. That is fine: the code holds no secrets, and no patient data is ever in the file.

### Why not Netlify

Netlify moved to a credit system for accounts created after September 2025 — 300 credits a month, with each deploy costing 15. That works out to roughly 20 deploys a month, and when the credits run out the site stops serving traffic entirely.

Bandwidth was never going to be an issue for one user, but the deploy cap bites while you are still making changes, and running out takes her app offline. GitHub Pages has no deploy cap.

**Cloudflare Pages** is a good alternative if you would rather not use a `github.io` address.

### Step 3 — Updating it later

Edit `index.html` on GitHub (pencil icon), commit, and Pages redeploys within a minute. She refreshes the app to get the new version. Her data is untouched by updates.

### Pick the URL before she starts

Storage is tied to the exact web address. Moving hosts later, or renaming the repository, gives her an app that opens completely empty — the old data is still there, but under an address the app no longer uses.

Decide on the host and the name **now**, before real data goes in.

---

## 3. Installing on her iPhone

**This must be done in Safari.** Chrome and Firefox on iPhone cannot install home screen apps properly, and the install is what protects her data (see section 6).

1. Open **Safari**. Not Chrome.
2. Go to your GitHub Pages URL.
3. Tap the **Share** button — the square with an arrow, at the bottom of the screen.
4. Scroll down the list, tap **Add to Home Screen**.
5. Edit the name if she wants — this is the label under the icon.
6. Tap **Add**.

The icon appears on her home screen. Tapping it opens the app full-screen with no Safari address bar.

**From now on she should open it from that icon, not from Safari.** They keep separate storage — opening the site in a Safari tab will look empty even when the home screen app is full of data.

---

## 4. How each part works

### Calendar

- Dots under a date show how many appointments that day, up to three.
- **Pink dot** = still pending. **Faded blue dot** = completed.
- Tap a date to select it and see that day's appointments below.
- **Tap the selected date again** to add an appointment for it.
- Today's date has a pink background. The selected date has a blue one.

### Adding an appointment

Date, time, and patient name are required. Notes are optional and free text.

The **+ New appointment** button at the bottom works from anywhere.

### Appointment cards

- **Checkbox** on the left — tick when the patient has been seen.
- **Edit** — change anything, including date and time.
- **Delete** — tap once to arm, tap again to confirm. It disarms itself after 3.5 seconds. This is deliberate: a single accidental tap can't delete anything.

### Overdue

If an appointment's time passes and it hasn't been ticked, the card turns pink and gets an **Overdue** flag. It stays in Pending — nothing disappears on its own. The Edit button becomes **Reschedule**.

The counter at the top right shows overdue count first, then pending, then "All clear".

### Caseload

Same appointments, flat list, grouped by date.

- **Pending** — everything unticked, soonest first. Overdue items float to the top.
- **Completed** — everything ticked, most recent first.

The split is driven entirely by the checkbox, never by the clock.

### Quota

Three levels deep.

**Level 1** — a ring showing overall percentage, "89 cases to go", then the four departments with progress bars.

**Level 2** — tap a department, see its procedures with counts like "Extractions with review — 3 / 20".

**Level 3** — tap a procedure. A big number counts down: **"17 more to go."** Type a patient name at the bottom, tap **Log**, and a dated card appears. The number drops to 16.

The Enter key works, and the cursor stays in the box, so several cases can be logged in a row.

Finished procedures turn green with a tick.

### Important: the Calendar and the Quota are separate

Ticking a patient as completed on the Calendar does **not** log a case in the Quota. She logs those herself in the Quota tab.

This was a deliberate choice. Linking them would mean tagging every appointment with a procedure type, which adds friction to every single booking. If she finds herself double-entering constantly, that's the thing to revisit.

---

## 5. Backup and restore

Tap the **gear** icon, top right.

### Export

Produces one file — `clinic-log-backup-2026-08-16.json` — containing every appointment and the entire logbook.

On her phone the iOS share sheet opens, so she can tap **Mail** and send it to herself in two taps. On a computer it downloads to the Downloads folder.

### Restore

Tap **Restore from a backup**, pick the file. It tells her what's inside before changing anything — "This backup holds 34 appointments and 61 logged cases, saved 16 August 2026" — then offers two options:

- **Add to what's here** — merges, skipping anything already present. Safe to run twice.
- **Replace everything** — wipes the phone's data and uses the file instead. For a new phone. Asks twice.

A file that isn't a valid backup is rejected without touching her data.

**How often:** after any big logging session, and definitely before changing phones or updating iOS.

---

## 6. Where the data lives — read this part

All data is stored in `localStorage`, in the browser, **on her phone only**. Nothing is uploaded. There is no server, no account, no cloud, no sync.

### What that means, plainly

**Safe:**
- Refreshing the page
- Closing the app
- Restarting the phone
- Updating the app on GitHub
- Being offline

**Destroys everything:**
- Clearing Safari history and website data
- **Deleting the home screen icon** — the data goes with it
- Getting a new phone (nothing transfers by itself)
- Using it in Private Browsing

**Looks empty but isn't:**
- Opening the site in Safari when she normally uses the home screen icon
- Opening it in Chrome
- Opening it on a different device

### The iOS 7-day rule

Safari deletes locally-stored data after seven days without interacting with a site. This is Apple's anti-tracking policy and it has killed data for plenty of web apps.

**Home screen apps are officially exempt.** Apple's tracking-prevention documentation states that the first-party domain of home screen web applications is exempt from the seven-day cap, and WebKit's engineers have said they'd treat any such deletion as a serious bug.

So: **installed to the home screen is exempt. A Safari bookmark is not.** This is the single most important reason to do the install properly.

Two caveats. Developers do still occasionally report data loss on installed apps — mostly people who weren't actually installed, plus some genuine older iOS bugs. And iOS can clear data when the phone is nearly full.

The app asks for persistent storage on startup, which reduces the risk further, but Apple doesn't document how that interacts with their policy, so treat it as a bonus rather than a guarantee.

**Practical version:** install to the home screen, don't delete the icon, export now and then. The export button is the real safety net.

---

## 7. Privacy

Patient names are health information. In Malaysia that falls under the PDPA.

What's in your favour: the data never leaves her phone. There's no server to breach, no third party, no analytics. That's a genuinely better privacy position than most commercial apps.

What to be careful about:

- **Don't post the URL publicly.** The site is public — anyone with the link can open the app. They'd see an *empty* app, since data is per-device, but there's no reason to advertise it.
- **Backup files contain real patient names.** Emailing one to herself puts that data in her mailbox. That's usually fine for personal use, but it's a copy she should know exists.
- **Check her workplace policy** before this becomes her system of record. Some hospitals require clinical records to stay in approved systems. This is a personal tracker, not a medical record — worth being clear about that distinction with anyone who asks.
- **If she loses the phone**, whoever has it can open the app. Her passcode is the only thing protecting it. A phone passcode is essential, which she almost certainly already has.

---

## 8. Things you can edit yourself

All near the top of the `<script>` section.

**Her name** — one line:
```js
const NAME = "Fattie";
```

**The daily lines** — the list right below `NAME`. Add, remove, or rewrite freely; it picks by date, so the count doesn't matter.

**Quota targets** — the `DEFAULT_QUOTA` block. Change a `target` number, add a procedure, add a whole department.

**One catch:** the quota structure is only read from the code the *first* time the app runs on a device. After that it reads from her phone's storage. Editing `DEFAULT_QUOTA` later will not change what's already on her phone. If requirements change mid-year, tell me and I'll write a proper update path — don't just edit the numbers and expect them to appear.

**Colours** — the `:root` block at the top of the `<style>` section. Everything derives from those variables. `--bg` is the main background; nudge it darker or lighter and the rest follows.

---

## 9. What it deliberately doesn't do

- **No notifications.** Push on iOS home screen apps requires a server. The free substitute is a Shortcuts automation: Shortcuts → Automation → Time of Day → 6pm daily → Open URL → her app link, with "Ask Before Running" off. It opens by itself and she sees the pending count.
- **No automatic call logging.** iOS gives no app access to the call log. This isn't a paywall, it's a platform restriction with no workaround.
- **No contacts access.** Same reason.
- **No sync across devices.** One phone, one dataset.
- **No automatic monthly email.** Needs a server that runs on a schedule.
- **No link to the hospital system.** Still possible if their patient records have per-patient URLs — ask their IT first.

---

## 10. If something goes wrong

| Symptom | Likely cause |
|---|---|
| App is empty | Opened in Safari or Chrome instead of the home screen icon |
| Delete button does nothing | Tap it twice — first tap arms it |
| Export does nothing | Blocked in a preview frame; works from the installed app |
| Date picker looks wrong | iOS renders these natively; varies by iOS version |
| Changes to the file don't show | Pages still deploying, or needs a hard refresh |
| Quota targets didn't update | Expected — see section 8 |

---

## 11. Changing the app later

Editing the file does **not** wipe her data. Storage lives in the browser, not in the file — swap `index.html` and her appointments and logbook stay put.

Safe to change freely: colours, fonts, wording, layout, new buttons, new views, bug fixes.

Three things that *do* destroy data:

1. **Changing the URL.** Storage is tied to the exact address. Moving hosts, renaming the repo, or switching domains gives her an empty app.
2. **Renaming the storage keys.** `A_KEY` and `Q_KEY` near the top of the script still say `rounds.` — leave them. Tidying them orphans everything saved.
3. **Assuming a new field exists on old records.** Add a field, and every existing record lacks it. Code that assumes it is there will break. The fix is a migration on load; there is a working example at the bottom of the script where old `procedure` values fold back into `note`.

**Before any update, have her tap gear → Export.** Thirty seconds, and it makes the change reversible.

---

## 12. If it outgrows this

The point at which local storage stops being enough is when she wants it on two devices, or when losing the phone would be a disaster rather than an inconvenience.

At that point the path is **Supabase** — free tier, real database, syncs across devices, survives phone loss. It adds a login screen, and it means patient names live on a third-party server, which brings the PDPA question back in a way it doesn't apply today.

Not worth doing until she's actually used this for a month or two. Let real annoyances drive the next version rather than guesses.

