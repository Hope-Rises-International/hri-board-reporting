## ⟳ SESSION-START REFRESH — read first, every session (mandatory)

Before doing ANY work in this session:

1. **Hard-pull this repo** so you are on the latest canonical state:
       git pull origin main
2. **Re-read this CLAUDE.md top to bottom.** It may have changed since your last session.
3. **Re-read the canonical rails** that govern every HRI architect and builder session:
       gh api "repos/Hope-Rises-International/hri-template-repository/contents/RESTRUCTURE-RULES.md?ref=main" --jq '.content' | base64 -d
   (Architect sessions: also read hri-architect/CLAUDE.md and its RESTRUCTURE-RULES mirror.)

**Mid-session:** do a SOFT check only —
       git fetch && git status -sb
— and tell the operator if you are behind. Do NOT hard-pull mid-session; it can clobber in-progress edits.

This block is mandatory and identical across every HRI repo. If it is missing from a repo's CLAUDE.md, add it and tell the operator — that repo had drifted off the rails.

---

# Claude Code — Project Instructions

## About this project

**HRI Board Reporting** — A GitHub Pages site hosting board-level reports for Hope Rises International's Board of Directors. Each monthly edition is a self-contained, dated HTML file (e.g., `board-update-june-2026.html`), surfaced via clickable cards on the `index.html` landing page (newest-first). The repo is public (required for GitHub Pages on free plan), with `robots.txt` blocking search engine indexing. Board members access via direct link only.

- **Stack:** Static HTML (no build step)
- **Hosting:** GitHub Pages — `https://hope-rises-international.github.io/hri-board-reporting/`
- **Publisher:** Kelly (operator) authors/adds each edition.
- **Update model:** Per the org-wide rails, the PR/approval gate is **PAUSED since 2026-06-10 (Bill)** — **direct commit + push to `main` is allowed**; no PR or CODEOWNER approval is required. Add a new dated `board-update-<month>-<year>.html` file plus a matching newest-first card in `index.html`, commit, and push to `main`. GitHub Pages rebuilds automatically; the Board Governance platform's `/updates` tab auto-discovers the new file within its 5-minute cache (no app redeploy).
- **If a push to `main` is rejected** (`GH006 ... protected branch`), residual branch protection / a ruleset is still live on this repo (not yet removed under the pause) — fall back to a PR, or ask Bill/Craig to remove the protection.
- **Filename convention:** `board-update-<month>-<year>.html` (lowercase, e.g. `board-update-june-2026.html`). The governance platform derives the title/date from this slug via regex `^board-update-(.+?)\.html$`, so the slug must be correct.
- **CODEOWNERS (for gate revival):** `* @ccalvert-hri @bschwanbeck-HRI` — Craig (primary) / Bekah (backup); self-approval is forbidden, so a PR author needs the other owner. Not enforced while the gate is paused; re-applies when the banner is removed from `hri-template-repository/RESTRUCTURE-RULES.md`.
- **Start date:** 2026-03-14

## Stack Learnings (canonical source)

Stack-level learnings live in ONE place:
- Repo: `Hope-Rises-International/hri-template-repository`
- File: `hri-stack-learnings.md`
- Read before any infrastructure, auth, deployment, or tooling work.
- Update directly via GitHub API when you discover a stack-level gotcha. See session-end protocol below.

Do NOT create a local `learnings.md` or `hri-stack-learnings.md` in this repo. If one exists, merge any unique content upstream and delete the local copy.

## Project knowledge

**[2026-03-14 | Bill | Initial board report setup]**
- **Decided:** GitHub Pages over Webflow for board reports. Webflow doesn't offer real auth on hidden pages (same link-only model), and keeping reports separate from the donor-facing site eliminates risk of accidental exposure. Can revisit if a proper board portal with login is needed later.
- **Decided:** Public repo with robots.txt disallow-all. Required for GitHub Pages on current GitHub plan. No sensitive content in the repo — just rendered HTML reports and a markdown draft.
- **Changed:** Created repo, deployed March 2026 board update as `index.html`. Also includes `board-report-2026-03.md` (an earlier markdown draft — the HTML version is the authoritative board deliverable).
- **Watch out:** Future editions just replace `index.html`. No versioning of old reports in the repo (user said "we'll just replace it"). If archiving past editions becomes needed, consider keeping them as dated files.

**[2026-04-14 | Bill | April board update + landing page]**
- **Decided:** Switched from single-file replacement to a card-based landing page at `index.html`. Each monthly update is now a separate dated HTML file (e.g., `board-update-april-2026.html`). Landing page has clickable cards with month title, snippet, and link. This replaces the prior model of overwriting `index.html` each month.
- **Changed:** Created April 2026 board update with edits to financials (net deficit featured at -$283K vs ~-$270K budget target, removed legacy and program ratio stat boxes), LepVax section (added program ratio pressure language from GHIT grant loss), and closing text (Bill's South Kivu / 708 people narrative). Renamed March report from `index.html` to `board-update-march-2026.html`. New `index.html` is the card-based landing page with HRI logo. Grid column count updated from 3 to 2 for the financials stat strip.
- **Watch out:** When adding future months, add a new card to `index.html` and a new dated HTML file. Cards should go newest-first. The stat-strip grid is now `repeat(2, 1fr)` — if a future report needs 3 stat boxes, update accordingly.

**[2026-06-15 | Kelly | June board update + branch protection / PR workflow]**
- **Changed:** Published the June 2026 board update (`board-update-june-2026.html`) — Paul Saunderson retirement lead story, embedded LepVax webinar executive summary (attributed to Board Chair Neal Joseph), year-end financial direction, FY2027 budget-approval notice, fundraising, and prayer requests. Added the newest-first June card to `index.html`.
- **Changed:** At publish time a direct `git push origin main` was rejected (`GH006: protected branch ... Changes must be made through a pull request`), so the June edition shipped via PR #1 (approved by Craig) and this doc change via PR #2.
- **Clarified (rails):** `hri-template-repository/RESTRUCTURE-RULES.md` carries a **GATE PAUSED banner dated 2026-06-10 (Bill)** — org-wide, direct push to `main` is allowed and branch protection + CODEOWNERS are being removed per-repo. So the intended model under the pause is **direct commit + push to `main`** (see "About this project"). This repo still reported `protected: true` mid-session (legacy `/protection` 404s → likely a residual ruleset); if a push is rejected, the protection on this repo just hasn't been lifted yet — ask Bill/Craig to remove it.
- **Publisher note:** Kelly is the regular operator who authors/adds board editions.
- **Watch out (macOS):** Finder seeds `Icon\r` files throughout the working tree, and a clone here picked them up inside `.git/refs/**`, producing `fatal: bad object refs/.../Icon?` and blocking `git fetch`/`pull`. Fix: delete every file whose name contains a carriage return under `.git/` (and the repo generally) before fetching. Consider adding `Icon?` to `.gitignore` / global excludes.

---

## Session Start

Before beginning work, check the most recent Project Knowledge entry below. If it contains an "Open" field, surface the open items to the user and confirm whether to continue that work or start something new.

---

## Session-End Protocol

**The full protocol lives in one place:** `session-end-protocol.md` in `hri-template-repository`.

At session close, fetch and follow it:

```bash
gh api /repos/Hope-Rises-International/hri-template-repository/contents/session-end-protocol.md \
  --jq '.content' | base64 -d > /tmp/session-end-protocol.md
```

Then read `/tmp/session-end-protocol.md` and execute all steps.

This ensures every repo uses the latest protocol without needing per-repo updates.
