# AGENTS.md — diary-site

Static landing page for `diary.horatio.cn`, the download entry for the 热量日记 and 开支日记 Android apps: one `index.html` with inline CSS, no build step and no external runtime resources. Push to `main` deploys it via self-hosted Actions rsync to `/www/wwwroot/diary.horatio.cn/`.

## Precedence

- Nearest `AGENTS.md` wins; an explicit user instruction wins over any file.
- When docs and code disagree, follow the code and report the inconsistency.

## Commands

| Purpose | Command |
| --- | --- |
| Build / test / lint | none — single static HTML file, no package or build config |
| Deploy | push `main` → `.github/workflows/deploy.yml`, or `workflow_dispatch` for a manual run |

The workflow confirms the target directory over ssh before rsync and uses `ci/deploy-managed.sh` with `ci/deploy-exclude.txt`; no test or build gate runs before deploy.

## Boundaries

**Always**
- Keep the page self-contained: no CDN scripts, external fonts or external images; fonts use the system stack, icons are inlined Lucide SVGs.

**Ask first**
- Editing `.github/workflows/deploy.yml` or `ci/deploy-exclude.txt` (they decide what production keeps or deletes).

**Never**
- Commit deploy secrets — they live only in repo Secrets (`SERVER_HOST` / `SERVER_USER` / `SERVER_SSH_KEY`).
- Expect panel/certificate files to be synced — `.user.ini`, `.htaccess`, `.well-known/*` and `AGENTS.md` are excluded and never touched. Files previously deployed but removed from the repo are deleted via the remote manifest (`~/.deploy-manifests/`), not rsync `--delete`.

General rules: `~/.config/opencode/AGENTS.md`.

## Pointers

- `README.md`, `.github/workflows/deploy.yml`, `ci/deploy-exclude.txt`; app download links point back to the two GitHub repos named in `index.html`.
