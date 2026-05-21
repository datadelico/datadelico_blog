# Copilot instructions — datadelico/datadelico_blog

Purpose
- Short, executable guidance for automated Copilot sessions working on this Hugo site (build, content conventions, where to look).

Build / test / lint (commands)
- Clone with submodules:
  - git clone --recurse-submodules <repo>
  - or after cloning: git submodule update --init --recursive
- Dev (preview):
  - hugo server -D --bind 0.0.0.0 --port 1313
- Build (production):
  - hugo --gc --minify
- Build including drafts (preview):
  - hugo -D --gc --minify
- Rebuild public output only:
  - rm -rf public/ && hugo --gc --minify
- Hugo Extended: required for SCSS/asset pipeline. Check with: hugo version (look for "Extended").
- Theme tooling (if present):
  - cd themes/hugo-coder && npm install && npm test (only if package.json exists in the theme)

Tests & linters
- No test framework detected in this repository (no package.json at repo root, no pyproject, no tests).
- No linting tooling detected. If tests or linters are added, include the single-test invocation here. Examples to document when present:
  - Node (theme): cd themes/hugo-coder && npm test -- <test-file-or-pattern>
  - Go: go test -run TestName ./...
  - Lint (JS): npm run lint -- <path/to/file>

High-level architecture
- Hugo static site (multilingual). Primary source: content/ (content/posts contains posts).
- config.toml controls languages (es default), taxonomies (category, series, tag, author), dateFormat, resourceDir (./resources).
- resources/ is used by Hugo Pipes (SCSS, image processing).
- static/ contains static assets copied to output.
- themes/hugo-coder is a git submodule providing templates, assets and optional tooling.
- public/ and top-level HTML/XML files are generated build artifacts — avoid editing them in source.

Key conventions (repo-specific)
- Create new content with Hugo archetype: hugo new posts/my-title.md (archetypes/default.md -> draft: true). Set draft: false to publish.
- Front matter typically includes: title, date, draft, type: post, category/series/tag/author.
- Use the dateFormat in config.toml when adding explicit date values.
- Place SCSS/asset sources under resources/ and use Hugo Extended for compilation.
- Update or switch the theme via git submodule; do not edit the submodule directly unless intentionally forking.
- Multilingual content: follow config.toml languages settings; default contentDir is content/ unless overridden per-language.

Files worth checking first
- config.toml
- archetypes/default.md
- content/posts/*
- resources/ (assets)
- themes/hugo-coder/ (after git submodule update)

Notes for automated Copilot sessions
- Ensure theme submodule is populated before building: git submodule update --init --recursive.
- Prefer editing Markdown under content/, not generated files in public/.
- If asset processing is introduced, require Hugo Extended and possibly Node/npm inside themes; check themes/hugo-coder/package.json.
- When adding tests or linters, document how to run a single test and where tests live.

AI assistant config files
- No repository-specific AI assistant config files detected (CLAUDE.md, .cursorrules, AGENTS.md, .windsurfrules, CONVENTIONS.md, AIDER_CONVENTIONS.md, .clinerules).

If you want this file expanded (example test commands, linter configs, or an MCP server setup for Playwright/Chromium), say which area to expand.
