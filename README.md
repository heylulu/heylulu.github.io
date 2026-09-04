# luliu.site — portfolio

Plain static site. No build step, no dependencies. Open `index.html` in a browser to preview locally.

```
index.html          home — hero, work cards, about, contact
awareness.html      PhD case study
publications.html   publication list
style.css           all styling; edit the tokens at the top of the file
favicon.svg
img/                figures exported from the thesis at 1600px
files/cv.pdf        currently a copy of CV-Lu.pdf — swap in whichever CV you want public
```

## Getting it live today

**Fastest route — Netlify Drop.** Go to `app.netlify.com/drop` and drag this whole folder onto the page. It is live on a `something.netlify.app` URL in about thirty seconds, no account needed to start. Then:

1. Claim the site (free account) so it doesn't expire.
2. Site settings → Domain management → Add custom domain → enter your domain.
3. Netlify shows you the DNS records. Add them at your registrar. Propagation is usually well under an hour.
4. HTTPS is issued automatically once DNS resolves.

To update later, drag the folder again — it replaces the whole site.

**Alternative — GitHub Pages.** Push this folder to a repo, Settings → Pages → deploy from `main` / root, then add the custom domain there. Better if you want version history; slower to set up the first time.

## Before you put the URL on your CV

- [ ] Open it on a phone. The layout is responsive, but read the case study on a small screen once.
- [ ] Check the publication list against Google Scholar — it was built from `CV-academic.tex`.
- [ ] Confirm HTTPS is active on the custom domain before the link goes anywhere.

`files/cv.pdf` is a copy of `CV-academic.pdf`. Replace that file whenever the CV changes.

## Editing

Colours, spacing and the max width are all CSS custom properties at the top of `style.css`. Changing `--accent` restyles every link, tag border and callout at once. Dark mode inherits from the same tokens — it follows the visitor's system setting.

Images are JPEGs exported at 1600px wide from the thesis PDFs. To regenerate one at a different size:

```
pdftoppm -jpeg -r 110 -scale-to-x 1600 -scale-to-y -1 -f 1 -l 1 source.pdf out
```

## Adding a case study

Copy `awareness.html`, change the `<title>`, the `.case-head` block and the body, then add a `<article class="card">` to `index.html`. Remove the `stub` class from a card once its case study exists and give it a real `card-media` image.
