# benbotsford.com

Personal site for Ben Botsford, a static page hosted on GitHub Pages with a custom domain from Squarespace Domains.

```
index.html                 # the whole site (HTML + inline CSS)
Ben_Botsford_Resume.pdf    # linked from the "Download résumé" button
CNAME                      # tells GitHub Pages the custom domain: benbotsford.com
```

## 1. Push to GitHub

Create a **public** repo named exactly `benbotsford.github.io`. Then:

```bash
cd benbotsford.github.io
git init -b main
git add .
git commit -m "Initial site"
git remote add origin git@github.com:benbotsford/benbotsford.github.io.git
git push -u origin main
```

In the repo, go to **Settings → Pages**. Set **Source** to *Deploy from a branch*, choose **Branch** `main` and folder `/ (root)`, then click Save.
The site will be live at https://benbotsford.github.io within a minute or two.

## 2. (Recommended) Verify the domain with GitHub

This keeps anyone else from claiming your domain on Pages.
Go to **GitHub → your avatar → Settings → Pages → Add a domain**, enter `benbotsford.com`, and GitHub will give you a TXT record like:

| Type | Host | Value |
|------|------|-------|
| TXT  | `_github-pages-challenge-benbotsford` | *(code GitHub shows you)* |

Add it in Squarespace (step 3), then click **Verify** back in GitHub.

## 3. DNS in Squarespace Domains

Go to **account.squarespace.com → Domains → benbotsford.com → DNS → DNS Settings**.

1. **Delete the Squarespace Defaults** preset record group if it's there. Those records point the domain at Squarespace's servers and will conflict with GitHub.
2. Add these **Custom Records**:

| Type  | Host | Data |
|-------|------|------|
| A     | `@`  | `185.199.108.153` |
| A     | `@`  | `185.199.109.153` |
| A     | `@`  | `185.199.110.153` |
| A     | `@`  | `185.199.111.153` |
| AAAA  | `@`  | `2606:50c0:8000::153` |
| AAAA  | `@`  | `2606:50c0:8001::153` |
| AAAA  | `@`  | `2606:50c0:8002::153` |
| AAAA  | `@`  | `2606:50c0:8003::153` |
| CNAME | `www`| `benbotsford.github.io` |

Leave any MX/email records alone if you use email on this domain.

## 4. Turn on the custom domain + HTTPS

Back in **repo → Settings → Pages**:

- **Custom domain** should already say `benbotsford.com` because the `CNAME` file sets it. If it doesn't, type it in and save.
- Wait for the DNS check to go green. This usually takes minutes, but it can take up to 24h.
- Check **Enforce HTTPS**. The checkbox unlocks once GitHub finishes issuing the certificate.

Check your DNS from a terminal:

```bash
dig benbotsford.com +noall +answer      # should show the four 185.199.x.153 IPs
dig www.benbotsford.com +noall +answer  # should CNAME to benbotsford.github.io
```

## Updating the site

Edit `index.html` (or swap in a new `Ben_Botsford_Resume.pdf`), commit, and push. GitHub Pages redeploys automatically.

## Troubleshooting

- **"Domain's DNS record could not be retrieved"**: DNS hasn't propagated yet, or the Squarespace Defaults records are still present.
- **Enforce HTTPS is greyed out**: wait up to an hour after DNS resolves. If it's still greyed out, remove the custom domain in Pages settings, save, and add it back to re-trigger the certificate.
- **404 at the custom domain**: make sure the `CNAME` file is on the `main` branch at the repo root.
