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

### Known Issues & Caveats

- The `postinstall` script runs `apt-get update && apt-get install -y ffmpeg` which requires root. Use `yarn install --ignore-scripts` if `ffmpeg` is already installed (it is pre-installed on the Cloud VM).
- `next.config.mjs` includes a `webpack.resolve.fallback` config to stub `fs`, `child_process`, `net`, `tls` on the client side. This is needed because `fluent-ffmpeg` (used only in `getStaticProps` in `src/pages/crafts/index.tsx`) imports Node.js-only modules. Without this config, the dev server and build both fail.
- No `.eslintrc.json` is committed to the repo. Running `next lint` or `next build` for the first time triggers an interactive ESLint setup prompt. The codebase has pre-existing lint errors (webkit video attributes, display name, etc.).
- Running `next build` while `yarn dev` is running will break the dev server because both write to `.next/`. Always stop the dev server before building.
- The build command `yarn build` works with `--no-lint` flag: `npx next build --no-lint`.

### System Dependencies

- **Node.js**: v22 (pre-installed)
- **ffmpeg**: Required for video processing in the crafts build pipeline (pre-installed at `/usr/bin/ffmpeg`)
