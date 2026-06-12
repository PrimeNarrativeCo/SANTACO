# SANTACO website — exact setup steps

Goal: site live on **GitHub Pages**, with a **visual editor at `/admin/`** that your team logs into with GitHub.

Because Netlify Identity is now deprecated, login is handled by a tiny free **Cloudflare Worker** (one-time setup). Editors will need a free GitHub account.

Replace `USERNAME` and `REPO` below with your own throughout.

---

## PART A — Put the site on GitHub  (~5 min)

1. Create a GitHub account at **github.com** (skip if you have one).
2. Click **＋ → New repository**. Name it `santaco-website`, set **Public**, click **Create repository**.
3. Unzip `santaco-cms.zip`. On the new repo page click **uploading an existing file**, then drag in **everything inside** the unzipped folder — `index.html`, the `admin` folder, the `data` folder, the `images` folder, `README.md`. Keep the structure (don't put them inside a subfolder). Click **Commit changes**.
4. Go to **Settings → Pages**. Under **Build and deployment**, Source = **Deploy from a branch**, Branch = **main** / **/ (root)**, **Save**.
5. Wait ~1 minute. Your site is live at **`https://USERNAME.github.io/santaco-website/`**. Open it to confirm.

---

## PART B — Create the login Worker (Cloudflare)  (~10 min, one time)

6. Sign up free at **cloudflare.com**.
7. Open **github.com/sveltia/sveltia-cms-auth** and use its **“Deploy to Cloudflare Workers”** button (top of that page). Sign in with Cloudflare and let it create the worker. (This worker works for Decap CMS too.)
8. In the **Cloudflare dashboard → Workers & Pages**, open the new `sveltia-cms-auth` worker and copy its URL — it looks like **`https://sveltia-cms-auth.YOURNAME.workers.dev`**. Keep it handy.

---

## PART C — Register a GitHub OAuth App  (~5 min)

9. On GitHub: your avatar → **Settings → Developer settings → OAuth Apps → New OAuth App**. Fill in:
   - **Application name:** `SANTACO CMS`
   - **Homepage URL:** `https://USERNAME.github.io/santaco-website/`
   - **Authorization callback URL:** `https://sveltia-cms-auth.YOURNAME.workers.dev/callback`
   - Click **Register application**.
10. Copy the **Client ID**. Click **Generate a new client secret** and copy that too.
11. Back in Cloudflare → your worker → **Settings → Variables and Secrets**, add two **secrets** (use the exact names the worker's README lists — for sveltia-cms-auth they are):
    - `GITHUB_CLIENT_ID` = the Client ID
    - `GITHUB_CLIENT_SECRET` = the client secret
    Save / redeploy.

---

## PART D — Point the editor at the Worker  (~2 min)

12. In your GitHub repo, open **`admin/config.yml`**, click the pencil to edit, and set the top **backend** block to:
    ```yaml
    backend:
      name: github
      repo: USERNAME/santaco-website
      branch: main
      base_url: https://sveltia-cms-auth.YOURNAME.workers.dev
    ```
    **Commit changes.**

---

## PART E — Give editors access & log in

13. Repo → **Settings → Collaborators → Add people** → add each editor's GitHub username. They accept the email invite. (Each editor needs a free GitHub account and this collaborator access — that's the trade-off now that the no-account Netlify login is gone.)
14. Go to **`https://USERNAME.github.io/santaco-website/admin/`** → **Login with GitHub** → **Authorize**. You're in.
15. Edit anything, click **Publish**. It commits to the repo and the live site updates in about a minute.

---

## Try it on your computer first (optional, no login needed)

`config.yml` already has `local_backend: true`, so you can preview the editor before doing any of the above:
1. Install **Node.js**.
2. In the unzipped folder run: `npx decap-server`
3. In a second terminal run: `npx serve .`
4. Open **http://localhost:3000/admin/** — edits save to your local files so you can see how it works.

---

## Notes

- View the site through the **github.io URL**, not by double-clicking the file — it loads its content over the web.
- If `/admin/` redirects to a Netlify address instead of GitHub, double-check the `base_url` in `config.yml` matches your Worker URL exactly.
- Maroon/yellow are still placeholder brand colours until you send exact hex values.
