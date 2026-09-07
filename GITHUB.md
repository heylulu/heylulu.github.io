# Publishing this site with GitHub Pages

Free, no build step, and you get version history. Two routes below — use the web route if you'd rather not touch a terminal.

**The repository must be public.** GitHub Pages from a private repo requires a paid plan (Pro or above). A public repo means anyone can read the source, which for a portfolio is fine — and arguably a small plus, since it shows you write your own HTML.

---

## Step 1 — Choose the repository name

This decides your URL, so pick before you create it.

| Repo name | Site URL |
|---|---|
| `heylulu.github.io` (your username, exactly) | `https://heylulu.github.io/` |
| anything else, e.g. `portfolio` | `https://heylulu.github.io/portfolio/` |

The first is a **user site** and gives you the clean root URL. Use that one — you only get one per account, and a portfolio is what it's for. Yours is `heylulu`, so the site is at `https://heylulu.github.io/`.

Either works with these files: every link in the site is relative, so nothing breaks at a subpath.

---

## Step 2a — Upload via the web (no terminal)

1. Sign in at **github.com**. Note your exact username.
2. Click **+** → **New repository**.
3. Name it `<username>.github.io`. Set **Public**. Do not tick "Add a README". Click **Create repository**.
4. On the empty repo page, click **uploading an existing file**.
5. Open the `site` folder on your Mac and drag **the contents** — `index.html`, `awareness.html`, `publications.html`, `style.css`, `favicon.svg`, `.nojekyll`, and the `img` and `files` folders — into the browser. Do **not** drag the `site` folder itself: `index.html` has to sit at the top level of the repo, not inside a subfolder.
6. Scroll down, click **Commit changes**.

macOS hides dotfiles in Finder, so `.nojekyll` will be invisible. Press **Cmd + Shift + .** in Finder to show hidden files, then include it. It stops GitHub's Jekyll processor from interfering.

## Step 2b — Or push from the terminal

```bash
cd ~/Desktop/CV-claude/site
git init
git add .
git commit -m "Portfolio site"
git branch -M main
git remote add origin https://github.com/<username>/<username>.github.io.git
git push -u origin main
```

GitHub will ask for a personal access token rather than a password. Create one at **Settings → Developer settings → Personal access tokens → Tokens (classic)** with the `repo` scope, and paste it when prompted.

---

## Step 3 — Turn Pages on

1. In the repo: **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)**. Click **Save**.
4. Wait one to two minutes. Reload the Pages settings page; your live URL appears at the top.

For a user site named `<username>.github.io`, this is often already on by default.

---

## Step 4 — Stay on the github.io domain

The site lives at **https://heylulu.github.io/** and there is **no custom domain**. Do not set
one in Settings → Pages, and do not add a `CNAME` file to the repo. Both do the same thing, and
the consequences below are why this is worth leaving alone.

**What happens if a custom domain is set.** GitHub immediately starts serving a permanent
redirect (HTTP 301) from `heylulu.github.io` to that domain. If the domain's DNS is not
already pointing at GitHub — which it will not be until records propagate, or ever, if the
domain was never registered — every page of the site becomes unreachable.

**Why removing it does not fix it straight away.** A 301 is a *permanent* redirect, and
browsers cache it on disk, per-site, for a long time. So after the custom domain is removed and
Pages is serving `heylulu.github.io` correctly again, your own browser can keep redirecting you
to the dead domain, while the site works perfectly for everyone else. Every link looks broken,
and a normal reload does not help because the browser never asks the server.

**How to clear it, if this has already happened:**

- Quickest check: open the site in a **private / incognito window**, which has no cached
  redirect. If it works there, the site is fine and only your normal browser is stale.
- Chrome: open `chrome://net-internals/#hsts`, put `heylulu.github.io` in **Delete domain
  security policies** and delete it; then clear cached images and files for the last hour.
- Safari: Develop → **Empty Caches**, or Settings → Privacy → Manage Website Data → remove
  `github.io`.
- Firefox: Settings → Privacy & Security → Cookies and Site Data → **Manage Data** → remove
  `github.io`.

---

## Updating later

**Web:** repo → **Add file** → **Upload files** → drag the changed files → Commit. Same-named files are replaced.

**Terminal:**
```bash
cd ~/Desktop/CV-claude/site
git add .
git commit -m "Update case study"
git push
```

Changes go live in about a minute. If you don't see them, hard-reload with **Cmd + Shift + R** — the old CSS is probably cached.

---

## If something looks wrong

**Page loads but is unstyled.** `style.css` didn't upload, or it's inside a subfolder. It must sit beside `index.html`.

**Images missing.** The `img` folder didn't come across. GitHub Pages is case-sensitive where macOS is not, so `Designs.jpg` and `designs.jpg` are different files there and identical here — keep everything lowercase.

**404 on the whole site.** Either Pages isn't enabled yet, `index.html` is one level too deep, or the first build hasn't finished. Check the **Actions** tab for a failed deployment.

**Every link suddenly goes nowhere, for you but not for others.** A custom domain was set on
Pages at some point and your browser cached the 301 redirect. See Step 4 for how to clear it.
Check first in a private window.
