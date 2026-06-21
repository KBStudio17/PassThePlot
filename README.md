# Pass the Plot — website

Three pages, sharing one stylesheet, in the same parchment/ink/brass visual identity as the app itself:

- `index.html` — home page. This is also what you put in the **Marketing URL** field in App Store Connect (Apple's term for a promotional page — it's not a separate policy document, just a link).
- `support.html` — for the **Support URL** field (required by both Apple and Google).
- `privacy.html` — for the **Privacy Policy URL** field (required by both Apple and Google).
- `styles.css` — shared design system (colors, type, layout).
- `favicon.svg` — small quill-and-ink mark used as the browser tab icon.
- `CNAME` — tells GitHub Pages which custom domain to serve this under.

## Before you publish

Search each file for these and fill in your real details:

- `support@passtheplotapp.com` and `privacy@passtheplotapp.com` and `hello@passtheplotapp.com` — these inboxes need to actually exist and be checked, or point them at one address you do check.
- `[Your Company Name]` in `privacy.html` — your legal entity or your own name if you're publishing as an individual.
- The "Last updated" date in `privacy.html` — update it whenever the policy actually changes.
- The App Store / Google Play badges in `index.html` — currently styled as "coming soon." Once you have real listing URLs, swap them for `<a>` links to your store pages.

**On the privacy policy specifically:** I've written a thorough, standard template covering the data the app actually collects based on what we designed (accounts, gameplay content, purchases via Apple/Google, device/usage data), children's privacy, retention, and user rights. It's a solid starting point, but it's not a substitute for a lawyer — especially before launch, it's worth a quick legal review to make sure it matches exactly what your analytics/crash-reporting SDKs collect and that the children's-privacy language fits your actual age positioning.

## Deploying with GitHub + Namecheap

**1. Push to GitHub**
```bash
cd passtheplot-site
git init
git add .
git commit -m "Pass the Plot site"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

**2. Turn on GitHub Pages**
- In the repo: Settings → Pages
- Source: "Deploy from a branch" → branch `main`, folder `/ (root)`
- Save. GitHub will give you a temporary URL like `https://<your-username>.github.io/<repo-name>/` — confirm the site loads there first.

**3. Point your custom domain at it**
- Still in Settings → Pages, under "Custom domain," enter `passtheplotapp.com` and save (this writes the `CNAME` file if it isn't already there — it already is, in this repo).
- In Namecheap, go to your domain's **Advanced DNS** tab and add:
  - Four `A` records for `@` pointing to GitHub's IPs:
    `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
  - One `CNAME` record for `www` pointing to `<your-username>.github.io`
- DNS changes can take a few minutes to a few hours to propagate.
- Back in GitHub Pages settings, once it detects the domain, check "Enforce HTTPS."

After that, `passtheplotapp.com`, `passtheplotapp.com/support.html`, and `passtheplotapp.com/privacy.html` are your three URLs for the app store listings.

## Notes

- No build step — it's plain HTML/CSS, so editing copy is just editing the `.html` files directly.
- Fonts (Fraunces, Spectral, Inter) load from Google Fonts via the `@import` at the top of `styles.css`.
- The home page's hero animation (the "context → reveal" demo) is pure CSS, plays once on load, and respects `prefers-reduced-motion`.
- `favicon.svg` works in all current browsers; if you want a PNG fallback for older devices, any "SVG to PNG" tool will do — export 32×32 and 180×180 versions and add the corresponding `<link>` tags in each page's `<head>`.
