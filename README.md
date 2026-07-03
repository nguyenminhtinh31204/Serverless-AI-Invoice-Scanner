# Serverless AI Invoice Scanner

Static Hugo documentation site for the `Serverless-AI-Invoice-Scanner` project.

Production URL:

```text
https://nguyenminhtinh31204.github.io/Serverless-AI-Invoice-Scanner/
```
```front-end connect to aws service
https://main.d1m4pmxvsvx5zk.amplifyapp.com/
```

## Structure

- `config/_default/hugo.toml`: Hugo site configuration.
- `content/`: English and Vietnamese workshop pages.
- `static/`: Images, CSS, fonts, and static assets.
- `layouts/`: Project-level layout overrides and shortcodes.
- `themes/hugo-theme-learn/`: Vendored Hugo Learn theme used by the site.
- `public/`: Generated build output, ignored by Git.

## Local Development

```powershell
hugo server -D
```

Open the local URL printed by Hugo, usually `http://localhost:1313/`.

## Production Build

```powershell
hugo --minify
```

The generated static website will be written to `public/`.

## GitHub Pages Deployment

Deployment is handled by `.github/workflows/deploy-pages.yml`.

Every push to `main` builds the Hugo site and publishes `public/` to GitHub Pages.