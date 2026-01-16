# Copilot instructions (mkdocs)

## Project overview
- This repo is a Material for MkDocs documentation site.
- Authoring source lives under `my-project/docs/`.
- `my-project/site/` is generated build output and is ignored (see `.gitignore`). Do not edit it directly.

## Core files and structure
- Site configuration and navigation are defined in `my-project/mkdocs.yml` (single source of truth for `nav`, theme, plugins).
- Python deps are pinned in `my-project/requirements.txt` (MkDocs + Material + pymdown extensions).
- Major section landing pages use `index.md` (example: `my-project/docs/azure-migrations/index.md`).

## Local dev workflow
Run from the `my-project/` directory:
- Install: `python -m pip install -r requirements.txt`
- Preview: `mkdocs serve`
- Build (matches CI): `mkdocs build --strict --clean`

## CI/CD and deployment
- GitHub Actions builds on PRs and builds+deploys on `main` pushes via `.github/workflows/mkdocs-pages.yml`.
- CI runs `mkdocs build --strict --clean`; broken links/nav or invalid config should be treated as build blockers.

## Writing and content conventions (observed)
- Use relative Markdown links between pages (example: `azure-migrations/reference/checklists.md` links to `glossary.md`).
- Checklists use GitHub-style task lists (`- [ ]`) and rely on `pymdownx.tasklist` in `mkdocs.yml`.
- Admonitions are supported (see `mkdocs.yml` extensions; example usage in `docs/index.md` with `!!! note`).
- Prefer not to hard-code Azure product limits that can change; when precision matters, call out what to verify in official Azure documentation (see `my-project/docs/index.md`).

## ADR conventions
- ADRs live in `my-project/docs/azure-migrations/adr/`.
- Follow the template in `my-project/docs/azure-migrations/adr/adr-template.md` and keep numbered filenames (e.g., `0001-...`).
- Keep the ADR `## Status` section and update it consistently when changing ADR state.

## Adding or moving pages
- If you add/move a page under `my-project/docs/`, update the `nav:` section in `my-project/mkdocs.yml` to match.
- Prefer adding new section landing pages as `index.md` and linking them from `nav`.

## Repo-specific guardrails
- Do not introduce new MkDocs plugins/markdown extensions unless you also update `my-project/requirements.txt` and `mkdocs.yml` together.
- Keep edits scoped to `my-project/` unless you are explicitly changing CI in `.github/workflows/`.

## Optional: agent role prompts
- Role-specific prompts live in `.github/agents/*.agent.md` (e.g., information architecture, technical writing, CI/CD).
