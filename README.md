# SANTACO website + content editor (Decap CMS)

This is the SANTACO site set up so that **statements, events, gallery pictures, the main images and the key wording can be edited through a visual editor** — no code. The editor saves changes straight to your GitHub repo, and the live site updates automatically.

```
/index.html            ← the website
/admin/                ← the content editor (Decap CMS)
   index.html
   config.yml
/data/                 ← the editable content (the CMS writes these)
   site.json           · wording + main images (hero, president, mission, vision)
   statements.json     · media statements
   events.json         · events
   gallery.json        · gallery photos
/images/uploads/       ← pictures you upload in the editor land here
```

The website reads those `/data/*.json` files when it loads and fills the page in. So when you change something in the editor, the website shows it.

> **Important:** because the page loads its content over the web, you must open it through a web address (GitHub Pages / Netlify), **not** by double-clicking the file. Opened locally it will fall back to the built-in default content.

---

## Step 1 — Put this on GitHub

1. Create a new repository (e.g. `santaco-website`).
2. Upload **all** the files and folders above, keeping the structure.
3. In **Settings → Pages**, set the source to your `main` branch. Your site is now live at `https://<username>.github.io/<repo>/`.

At this point the website works. The **editor** needs a login, which is the next step.

---

## Step 2 — Turn on the editor (choose ONE)

The editor needs a way to log people in. Pick whichever suits you.

### Option 1 — Netlify (recommended, easiest, free)

This is the simplest way to get a real "log in and edit" experience.

1. Go to **netlify.com**, sign up, and choose **Add new site → Import from GitHub**, and pick your repo. (Netlify just hosts the same GitHub files — your code stays on GitHub.)
2. In the new Netlify site: **Site configuration → Identity → Enable Identity**.
3. Under **Identity → Registration**, set it to **Invite only** (so only your team can log in).
4. Under **Identity → Services → Git Gateway**, click **Enable Git Gateway**.
5. Click **Invite users** and invite the email addresses of whoever will edit the site. They'll get an email to set a password.
6. Done. Go to `https://<your-netlify-site>/admin/` and log in.

`config.yml` is already set for this (`backend: git-gateway`).

### Option 2 — Stay on GitHub Pages only

GitHub Pages can't log people in by itself, so you deploy a tiny free "OAuth proxy" once:

1. Create a **GitHub OAuth App** (GitHub → Settings → Developer settings → OAuth Apps).
2. Deploy a small OAuth proxy — a well-known free one is the Cloudflare Workers / Render version of `decap-cms-github-oauth` (search "Decap CMS GitHub OAuth provider"). It takes ~10 minutes.
3. In `admin/config.yml`, comment out the `git-gateway` block and uncomment the `github` block, filling in your repo and the proxy's address.

If that sounds like a lot, use **Option 1** — it avoids all of it.

---

## Step 3 — Edit content

Go to **`/admin/`** on your live site and log in. You'll see four sections:

- **Wording & Main Images** — the homepage headline, the President's message, mission, vision, the main hero image and the President's photo. Change the text in the boxes, or click an image field to **upload a new picture**, then **Publish**.
- **Media Statements** — click **Statements**, then **＋ Add Statement**. Fill in the title, date, category, summary and (optionally) upload an image. Drag items to reorder. **Publish**.
- **Events** — same idea: **＋ Add Event**, set the status, date, title, location and detail line.
- **Gallery Pictures** — **＋ Add Photo**, upload an image, add a caption.
- **National Executive** — the nine office-bearers. Edit each one's **name, title, province and photo**.
- **Provincial Councils** — each province's **chairperson, seat, description and photo**. (The province *name* is a fixed dropdown so the interactive map keeps working — change the chairperson and the rest freely.)

Every **Publish** commits to GitHub and the live site updates within about a minute.

### Photos for leaders, provinces and the President
All editable through the editor now: the President's photo is in **Wording & Main Images**, the nine leaders' photos are in **National Executive**, and each province's photo is in **Provincial Councils**. Where a photo is left blank, the site shows a tidy initials/placeholder instead.

---

## Testing on your own computer first (optional)

1. Install Node.js.
2. In this folder run: `npx decap-server`
3. In another terminal serve the site, e.g. `npx serve .`
4. Open `http://localhost:3000/admin/` — `local_backend: true` lets you edit without any login. Changes save to your local files so you can see exactly how it works before going live.

---

## Notes

- The maroon/yellow colours are still approximations — send the exact brand hex values (or the brand guide) to lock them in.
- The default images currently load from SANTACO's existing website. Once you upload your own via the editor, those uploaded copies (in `/images/uploads/`) are used instead, which is more reliable.
