# Irina Zheleznova portfolio

Vue 3 + Vite portfolio prepared for GitHub Pages.

## Local development

```bash
npm install
npm run dev
```

## Content

- Main content and project data: `src/data.js`
- Page markup: `src/App.vue`
- Project modal: `src/components/ProjectModal.vue`
- Styles: `src/style.css`
- Required image exports: `public/portfolio/README.md`

Missing images are replaced by neutral placeholders. Add WebP files with the exact names listed in
`public/portfolio/README.md`; no code changes are required.

## GitHub Pages

The workflow `.github/workflows/deploy.yml` builds and publishes the site after every push to `main`.
It uses the current repository name as the Vite base path.

One-time repository setting:

1. Open GitHub repository `Settings` -> `Pages`.
2. Under `Build and deployment`, set `Source` to `GitHub Actions`.

Then publish changes:

```bash
git add .
git commit -m "Rebuild portfolio"
git push
```

The site URL will be:

```text
https://aa-pumpkin.github.io/ira-portfolio/
```

## Fork for Irina

1. Fork this repository to Irina's GitHub account.
2. Rename the fork to `zheleznova` in `Settings` -> `General`.
3. Open `Actions` and enable workflows for the fork.
4. Open `Settings` -> `Pages` and select `GitHub Actions` as the source.
5. In `Actions`, run `Deploy portfolio to GitHub Pages` once with `Run workflow`.

The fork will publish to:

```text
https://IRINA_USERNAME.github.io/zheleznova/
```

The workflow in the fork checks out `aa-pumpkin/ira-portfolio@main`, so it deploys the latest version
from the original repository every 15 minutes. Scheduled workflows in public forks are disabled by
GitHub until the fork owner enables them in the Actions tab.
