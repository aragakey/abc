# AGENTS.md

## Cursor Cloud specific instructions

### Project Overview

This is a Next.js 13 (Pages Router) portfolio/blog site called "设计垂点" by Aragakey. It uses static export (`output: "export"`) with `basePath: "/abc"`. There is no backend, database, or external service.

### Development

- **Package manager**: Yarn (v1, based on `yarn.lock`)
- **Dev server**: `yarn dev` starts both the Next.js dev server and a Babel content watcher via `concurrently`
- **Dev URL**: `http://localhost:3000/abc` (note the `/abc` basePath)
- **Lint**: `yarn lint` (requires an `.eslintrc.json`; the repo does not include one, so `next lint` will prompt interactively on first run)
- **Build**: `yarn build` runs `yarn build:content && next build`

### Known Issues

- The `postinstall` script runs `apt-get update && apt-get install -y ffmpeg` which requires root. Use `yarn install --ignore-scripts` if `ffmpeg` is already installed (it is pre-installed on the Cloud VM).
- The `/abc/crafts` page fails to compile in both dev and build modes because `fluent-ffmpeg` (a Node.js-only module) is imported at the top level of `src/pages/crafts/index.tsx`. This is a pre-existing code issue. All other pages (homepage, article pages) work correctly in dev mode.
- `yarn build` also fails because the codebase has pre-existing ESLint errors (e.g., `react/no-unknown-property` for webkit video attributes). The CI workflow in `.github/workflows/deploy.yml` is present but there are no recorded CI runs.
- No `.eslintrc.json` is committed to the repo. Running `next lint` or `next build` for the first time triggers an interactive ESLint setup prompt.

### System Dependencies

- **Node.js**: v22 (pre-installed)
- **ffmpeg**: Required for video processing in the crafts build pipeline (pre-installed at `/usr/bin/ffmpeg`)
