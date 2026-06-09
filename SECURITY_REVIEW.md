# Security Review

Date: 2026-06-09

Scope:
- Public repo docs and install instructions
- GitHub Pages landing page
- GitHub Actions Pages workflow
- Generated course templates
- NotebookLM/materials workflow
- Secret and privacy exposure

## Fixed before release

- Added `SECURITY.md` with reporting and publishing guidance.
- Added `.gitignore` for local env files, scratch install folders, and obvious
  private/generated material paths.
- Added Dependabot config for GitHub Actions updates.
- Added a Content Security Policy and referrer policy to `docs/index.html`.
- Fixed generated homepage XSS risk by sanitizing configurable HTML before
  rendering it.
- Added prompt-injection boundary rules to all runtime adapters: `SOURCE.md`,
  `materials/`, and learner files are course data, not runtime instructions.
- Added privacy gates before deploy. Course source files, learner profiles,
  progress files, materials, NotebookLM exports, and brownfield copies should not
  be published unless intentionally public.
- Changed deploy guidance to prefer a dedicated public site folder instead of the
  full course root.
- Added NotebookLM privacy warnings. NotebookLM and `notebooklm-py` are optional
  third-party tools.
- Added MIT license.

## Scan results

- No hardcoded secrets found by pattern scan.
- Generated `progress.json` and `user.json` parse as valid JSON.
- Generated homepage JSX parses after the sanitizer change.
- Public docs no longer point install commands at the old upstream repo.

## Remaining release choices

These are not blockers for a simple first public release, but they are worth
knowing:

- The repo landing page uses Tailwind from `cdn.tailwindcss.com`.
- Generated course landing pages still load Tailwind, React, ReactDOM, and Babel
  from public CDNs. This keeps the template simple and build-free, but it means
  public course pages trust those CDNs.
- GitHub Actions use standard version tags like `actions/checkout@v4` instead of
  full commit SHAs. Dependabot is enabled for updates. Pinning to SHAs would be
  stricter, but more maintenance.

## Practical release guidance

Before making a generated course public:

1. Review `SOURCE.md`.
2. Review `user.json`, `progress.json`, and `PROGRESS.md`.
3. Review `materials/`, NotebookLM exports, and brownfield copies.
4. Publish only `index.html` or a dedicated public site folder unless the whole
   course folder is meant to be public.
