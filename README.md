# Daily Riddle — 15-player activity edition

A Node.js 22+ and SQLite daily-riddle website for GitHub Codespaces. No `npm install` dependencies required.

## Run in Codespaces

In the folder containing `package.json`, run each command separately:

```bash
export ADMIN_PASSWORD='choose-a-long-private-password'
export SESSION_SECRET="$(openssl rand -hex 32)"
npm start
```

In **Ports**, forward port **3000** and set its visibility to **Public** if sharing with friends. Open its forwarded URL. The admin page is at `/admin.html`. Do not share the admin password. The Codespace and server must remain running for the link to work. This is development hosting, not reliable 24/7 hosting.

If using a public HTTPS forwarded port, you may also set `export COOKIE_SECURE=true` before `npm start`. If testing through plain HTTP localhost, leave it unset.

## Configure

1. Log in at `/admin.html`.
2. Under **Registered players**, enter **exactly 15 unique names**, one per line, and save. Players see them in alphabetical order.
3. Create today's riddle using the UTC date displayed on the player page and add accepted answers, one per line.
4. Share the public forwarded URL (not the admin URL).
5. In **Player results**, click **View timeline** for any player to see start/end timestamps, tab hidden/visible events with away durations, first typing, copy events, both submitted answers, and Solved/Not solved.

A player has two answer submissions. A correct answer on either try finishes as Solved. Two wrong submissions finish as Not solved. The player name is reserved for the day as soon as the challenge begins; a saved browser session can resume if not finished. Names are not authenticated, so anyone with the link could impersonate another player.

## Updating an earlier version

Back up your `data/` folder, then replace `server.js`, `public/index.html`, `public/app.js`, `public/admin.html`, `public/admin.js`, and `public/style.css` with this edition. **Do not overwrite your existing `data/` folder.** Restart the server with Ctrl+C and `npm start`. New database tables are created automatically. Existing completed attempts remain, but older versions did not collect full event timelines or two individual answers, so those historical details cannot be reconstructed.

The **Clear attempts for selected date** admin control permanently deletes all attempts, submitted answers, and activity events for that day. It then allows editing that day's riddle. This cannot be undone. Save a backup first if needed.

## Tracking accuracy and privacy

The browser reports visibility and interaction events, and the server timestamps them when received. A user can close the browser, lose connectivity, disable JavaScript, or tamper with client-side event reporting; therefore the timeline can be incomplete and is **not evidence of cheating**. Copy events are detected for text selected from the displayed riddle using the browser's copy event; screenshots, manual transcription, and some browser methods cannot be detected. The first-typing event captures only the first input, not individual keystrokes. Tell players about this tracking before they begin.

The admin dashboard is password-protected, but the player roster is visible to anyone with the public site URL. This is a lightweight game, not a secure identity or anti-cheating platform. Avoid using sensitive names or personal information. For persistent hosting, configure `DATA_DIR` to a persistent directory and back up the SQLite database.

## Files

- `server.js` — HTTP API, SQLite persistence, server timestamps and scoring
- `public/index.html`, `public/app.js` — player UI
- `public/admin.html`, `public/admin.js` — private dashboard
- `public/style.css` — shared styling
- `data/riddles.sqlite` — database generated on first run (ignored by Git)
