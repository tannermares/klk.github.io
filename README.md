# klk.github.io

The marketing site and legal pages for **klk** — a private, invite-only app for sharing
photos and video inside small groups.

The app itself lives in a separate repo (`klk`). This one is only the public web presence.

## Pages

| File | What it is |
| --- | --- |
| `index.html` | Landing page — hero, illustrated app mockups, the case for klk |
| `privacy.html` | Privacy Policy — **required** by App Store Connect for submission |
| `terms.html` | Terms of Service |
| `styles.css` | The whole stylesheet, shared by all three pages |
| `favicon.svg` | The shutter mark |

## Running it

Static HTML with no build step, no dependencies, and no package manager. Open the files
directly, or serve the folder so relative links behave exactly as they will in production:

```bash
python3 -m http.server 8765   # then open http://localhost:8765/
```

## Deploying

GitHub Pages serves whatever is on `main`. Push and it is live at **klkapp.com** — the
`CNAME` file sets the custom domain, so leave it in place (Pages rewrites it from the repo
settings, and deleting it drops the domain).

The apex domain needs these DNS records at the registrar. `www` is optional but worth
adding so both spellings resolve.

```
A     @     185.199.108.153
A     @     185.199.109.153
A     @     185.199.110.153
A     @     185.199.111.153
CNAME www   tannermares.github.io.
```

Then in the repo's **Settings → Pages**, confirm the custom domain and tick **Enforce
HTTPS** once the certificate is issued (it can take an hour or so after DNS propagates).

## Contact addresses

`privacy@klkapp.com` handles data-rights requests — deletion, access, export.
`support@klkapp.com` handles everything else, and is the footer link on every page.
