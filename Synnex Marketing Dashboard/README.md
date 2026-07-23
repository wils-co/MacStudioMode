# Publishing the Synnex Marketing Dashboard to GitHub

This guide sets up the dashboard so your team can view it at a live URL, and so it
**auto-pushes to GitHub every time the 9:30am refresh updates it**.

Confirmed working: git is available in the refresh environment and can reach GitHub,
so the automatic push is genuinely possible (no manual upload needed after setup).

---

## Part A — One-time GitHub setup (you do this)

1. **Create an empty repo** at github.com — e.g. `synnex-dashboard`.
   Don't add a README or any files (keep it empty so the first push is clean).

2. **Create a Personal Access Token** so the scheduled task can push without you logging in:
   - GitHub → Settings → Developer settings → **Fine-grained tokens** → **Generate new token**
   - **Repository access:** only the `synnex-dashboard` repo
   - **Permissions:** Contents → **Read and write**
   - Set an expiry you're comfortable with, generate, and **copy the token** (starts with `github_pat_…`)
   - The single-repo, fine-grained scope means a leaked token can only affect this one dashboard.

3. **Enable GitHub Pages:**
   - Repo → Settings → **Pages**
   - Source: **Deploy from a branch** → branch `main` → folder `/ (root)` → Save
   - After the first push, your live link will be:
     `https://<your-username>.github.io/synnex-dashboard/`

---

## Part B — Wire up the folder (one-time)

Run these in the `Synnex Marketing` folder. The file is renamed to `index.html`
because GitHub Pages serves that automatically.

```bash
cd "Synnex Marketing"
git init -b main
git mv Marketing_Dashboard.html index.html
git remote add origin https://<TOKEN>@github.com/<your-username>/synnex-dashboard.git
git add -A
git commit -m "Initial dashboard"
git push -u origin main
```

- `<TOKEN>` = the token from Part A; `<your-username>` = your GitHub username.
- The token is stored in the folder's `.git/config` on your machine so future pushes
  are automatic. It's plaintext there — fine for your own computer, just don't share the folder.

---

## Part C — Auto-push on every refresh

Add this as the **final step** of the 9:30am scheduled task, so right after it updates
the dashboard it commits and pushes:

```bash
cd "Synnex Marketing"
git add -A
git diff --cached --quiet || (git commit -m "Auto-refresh $(date +%F)" && git push)
```

- The `git diff --cached --quiet ||` guard means it **only commits/pushes when something
  actually changed** — quiet days won't create empty commits.
- The live Pages site updates within ~1 minute of each push.

> Note: because the file is renamed to `index.html`, the scheduled task and any references
> should point at `index.html` instead of `Marketing_Dashboard.html`.

---

## Updating later

After setup, you never touch GitHub manually — each refresh pushes itself. To change the
design or content by hand, edit `index.html`, then run the Part C commands (or just let the
next scheduled run push it).

## Alternatives (no GitHub)

- **Netlify Drop** (https://app.netlify.com/drop): drag the HTML file in, get an instant
  public link, no account needed. Re-drag to update.
- **Email the file**: it's fully self-contained and opens in any browser — but won't
  auto-update.
