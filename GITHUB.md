# Publishing this site with GitHub Pages

Free, no build step, and you get version history. Two routes below — use the web route if you'd rather not touch a terminal.

**The repository must be public.** GitHub Pages from a private repo requires a paid plan (Pro or above). A public repo means anyone can read the source, which for a portfolio is fine — and arguably a small plus, since it shows you write your own HTML.

---

## Step 1 — Choose the repository name

This decides your URL, so pick before you create it.

| Repo name | Site URL |
|---|---|
| `luliu.github.io` (your username, exactly) | `https://luliu.github.io/` |
| anything else, e.g. `portfolio` | `https://luliu.github.io/portfolio/` |

The first is a **user site** and gives you the clean root URL. Use that one — you only get one per account, and a portfolio is what it's for. Replace `luliu` with whatever username you register.

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

## Step 4 — Point your custom domain at it

Once you've registered the domain.

**In GitHub:** Settings → Pages → **Custom domain** → type `luliu.design` (or whichever) → **Save**. This writes a `CNAME` file into your repo automatically.

**At your registrar,** add DNS records. Values confirmed against GitHub's documentation:

For the apex domain (`luliu.design`), four **A** records, all with host `@`:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Optionally add four **AAAA** records for IPv6, same host:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

For `www`, one **CNAME** record with host `www` pointing to `<username>.github.io` — your username, no repository name, with the trailing dot if your registrar wants one.

**Then wait.** DNS usually resolves within an hour. GitHub issues the HTTPS certificate automatically once it does; when the **Enforce HTTPS** checkbox on the Pages settings page becomes clickable, tick it. That can take up to 24 hours, occasionally longer. The site works over plain HTTP in the meantime, but do not put the URL anywhere until HTTPS is live — a browser warning on a portfolio link is worse than no link.

---

## The gotcha that will bite you

Setting a custom domain creates a `CNAME` file **in the repo, not on your Mac**. If you later re-upload the whole folder through the web interface, that file disappears and your domain stops working.

Fix it once, now: after Step 4, create a plain text file called `CNAME` (no extension) in your local `site` folder containing exactly one line — your domain, e.g. `luliu.design` — and keep it there. Then every future upload carries it along.

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

**Custom domain shows someone else's site or an error.** DNS hasn't propagated. Give it an hour before changing anything — repeatedly editing records restarts the clock.
