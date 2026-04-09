# The Privee Group — Lead Tracker Setup Guide

The application has been migrated from Google Apps Script / Google Sheets to a Supabase backend. Follow these steps to set it up or modify the setup.

---

## Step 1 — Local Development

1. Open `index.html` in your browser.
2. The application is now fully client-side and connects directly to the Supabase database.
3. No configuration prompt is required anymore: the Supabase URL and Anon Key are hardcoded into the script in `index.html`.

---

## Step 2 — Supabase Backend

1. The data is securely stored in a Supabase PostgreSQL database.
2. The project contains a `leads` table tracking all the data.
3. Data fields map accurately from the previous Google Sheet structure.

---

## Step 3 — Deploy to Netlify (optional)

1. Go to netlify.com and sign up / log in.
2. Drag and drop **index.html** onto the Netlify deploy area.
3. Netlify gives you a URL like `https://your-site.netlify.app`.
4. Share that URL with Mayank and anyone else on the team.
5. Setup is instant and doesn't require pasting any URL.

---

## How it works day-to-day

- **On every refresh or "↻ Sync" click:** the tracker reads the latest data from Supabase.
- **Every time you add or update a lead:** it writes to Supabase in real time.
- **Everyone sees the same data** because it all lives in one DB.

---

## Troubleshooting

**"Sync error" message in the tracker**
- Check your internet connection.
- Ensure the Supabase project isn't paused due to inactivity.

**Changes the Code**
- Modify `index.html` locally and upload it to your host.