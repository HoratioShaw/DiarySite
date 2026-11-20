# AGENTS.md — DiarySite

Static landing page for `diary.horatio.cn`, the download entry for the 热量日记 / 开支日记 Android apps: one `index.html` with inline CSS, no build step or external runtime resources. Push to `main` runs `.github/workflows/deploy.yml` manifest-managed rsync to `/www/wwwroot/diary.horatio.cn/`.

## Commands

| Purpose | Command |
| --- | --- |
| Build / test / lint | none — single static HTML file, no package or build config |
| Deploy | push `main` → `.github/workflows/deploy.yml`, or manual `workflow_dispatch` |
| Target check | the workflow runs `ssh … 'ls -A /www/wwwroot/diary.horatio.cn'` before rsync |

The workflow confirms the target directory over ssh before rsync and uses `ci/deploy-managed.sh` with `ci/deploy-exclude.txt`; no test or build gate runs before deploy.

## Context loading

| Task | Read |
| --- | --- |
| Deploy behavior / exclusions | `.github/workflows/deploy.yml`, `ci/deploy-managed.sh`, `ci/deploy-exclude.txt` |
| Page copy / download links | `index.html`, `README.md` |

## Boundaries

### Always
- Keep the page self-contained: no CDN scripts, external fonts or external images; fonts use the system stack, icons are inlined Lucide SVGs.

### Ask first
- Editing `.github/workflows/deploy.yml` or `ci/deploy-exclude.txt` (they decide what production keeps or deletes).

### Never
- Commit deploy secrets — they live only in repo Secrets (`SERVER_HOST` / `SERVER_USER` / `SERVER_SSH_KEY`).
- Expect panel/certificate files to sync — `.user.ini`, `.htaccess`, `.well-known/*` and `AGENTS.md` are excluded and never touched. Files previously deployed but removed from the repo are deleted via the remote manifest (`~/.deploy-manifests/`), not rsync `--delete`.

General rules: `~/.config/opencode/AGENTS.md`.
