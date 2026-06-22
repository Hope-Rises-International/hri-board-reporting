## ⟳ SESSION-START REFRESH — read first, every session (mandatory)

Before doing ANY work in this session:

1. **Hard-pull this repo** so you are on the latest canonical state:
       git pull origin main
2. **Re-read this CLAUDE.md top to bottom.** It may have changed since your last session.
3. **Pull the canonical standards once — one shallow clone, then read from it** (this is what realizes any new or changed standard automatically; the standards are single canonical copies, no local mirror to drift):
       rm -rf /tmp/hri-template && gh repo clone Hope-Rises-International/hri-template-repository /tmp/hri-template -- --depth 1
   **Fail closed:** this clone is the ONLY source of the rails (no mirror fallback). If it fails or `/tmp/hri-template/RESTRUCTURE-RULES.md` is absent, **STOP** and tell the operator — do not work without the rails. Then from `/tmp/hri-template`: **read `RESTRUCTURE-RULES.md` every session** (the canonical rails). Before writing **any** integration code, also read `hri-integration-standards.md` + `hri-stack-learnings.md` there. The session-start / session-end / comprehension protocols live in the same clone. If you see a standards doc you don't recognize, read it and tell the operator. (Architect sessions also read `hri-architect/CLAUDE.md`, the Operating Manual.)

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
- **Update model:** Add a new dated `board-update-<month>-<year>.html` file plus a matching newest-first card in `index.html`, commit, and **push directly to `main`**. GitHub Pages rebuilds automatically; the Board Governance platform's `/updates` tab auto-discovers the new file within its 5-minute cache (no app redeploy). *(The PR/CODEOWNER gate is paused org-wide per `hri-template-repository/RESTRUCTURE-RULES.md` — banner dated 2026-06-10 — and this repo's branch protection was removed 2026-06-16, so direct push works. If a push is ever rejected with `GH006 ... protected branch`, protection was re-enabled: fall back to a PR approved by a CODEOWNER.)*
- **Filename convention:** `board-update-<month>-<year>.html` (lowercase, e.g. `board-update-june-2026.html`). The governance platform derives the title/date from this slug via regex `^board-update-(.+?)\.html$`, so the slug must be correct.
- **CODEOWNERS (only if the gate is revived):** `* @ccalvert-hri @bschwanbeck-HRI` — Craig (primary) / Bekah (backup); self-approval is forbidden, so a PR author needs the other owner. Not enforced while the gate is paused; re-applies if branch protection is restored and the banner is removed from `hri-template-repository/RESTRUCTURE-RULES.md`.
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
- **Changed:** `main` is now **branch-protected** — direct `git push origin main` is rejected (`GH006: protected branch ... Changes must be made through a pull request`). All edits now go through a PR approved by a CODEOWNER (`@ccalvert-hri` primary, `@bschwanbeck-HRI` backup). Updated "About this project" above to document the current PR-based publish workflow; the old "replace `index.html` and push to main" model no longer applies.
- **Publisher note:** Kelly is the regular operator who authors/adds board editions. Because GitHub forbids self-approval, Kelly's PRs must be approved by Craig or Bekah before merge.
- **Watch out (macOS):** Finder seeds `Icon\r` files throughout the working tree, and a clone here picked them up inside `.git/refs/**`, producing `fatal: bad object refs/.../Icon?` and blocking `git fetch`/`pull`. Fix: delete every file whose name contains a carriage return under `.git/` (and the repo generally) before fetching. Consider adding `Icon?` to `.gitignore` / global excludes.

**[2026-06-16 | Kelly | LepVax spotlight, August poll banner, gate removed]**
- **Decided:** Added a reusable time-sensitive board-action component to the June edition — a Rising Sun `.action-bar` (eyebrow + message + white button) for the August board-call scheduling poll (Rallly). After trying it under the masthead, settled it directly above the "From the Field" section, then bumped its font for prominence. Also added the LepVax webinar executive-summary "Strategic Spotlight" card (embedded `.docx` download + YouTube link), attributed to Board Chair Neal Joseph and framed for directors who couldn't attend the webinar.
- **Changed:** `board-update-june-2026.html` only (the `.action-bar` pattern lives in that file; copy it forward for future editions). Shipped via **direct push to `main`**.
- **Watch out:** **Branch protection on this repo was removed** — `git push origin main` now succeeds (confirmed 2026-06-16). The org-wide PR/CODEOWNER gate is paused per `hri-template-repository/RESTRUCTURE-RULES.md` (banner dated 2026-06-10). The "About this project" section and the 2026-06-15 entry above still say "branch-protected — no direct pushes" / "CODEOWNER approval required" — that wording is now **stale**. The macOS `Icon\r`-in-`.git/refs` fetch-blocker still recurs (now also lifted to `hri-stack-learnings.md`).
- **Open:** Correct the stale "no direct pushes" wording in "About this project," and decide what to do with the open doc-fix **PR #3** (predates Bill's recent CLAUDE.md edits; now stale). Separately, in `hri-board-governance`: directors must be provisioned (Admin → 10.4 Roster management, which mints the Clerk invite) and sent the from-Bill welcome email before they can view this update/poll on the platform — not a task in this repo.

**[2026-06-22 | Kelly | Poll link → Google Form; gate retired; docs squared away]**
- **Changed:** Swapped the August board-call poll button from Rallly to the Google Forms availability poll (clean `/viewform` link). Corrected "About this project" to the direct-push model and closed the now-stale doc PR #3 — this **resolves the first two Open items from the entry above**.
- **Watch out:** The PR/CODEOWNER gate is now **retired** org-wide (Bill, effective 2026-06-18), not merely paused — direct commit + push to `main` is the standing model, and `.github/CODEOWNERS` was deleted from the repo. The macOS `Icon\r`-in-`.git/refs` fetch-blocker still recurs each session; clear carriage-return-named files under `.git/` before fetch (documented in `hri-stack-learnings.md`).
- **Open:** (Cross-repo, not here) In `hri-board-governance`, directors still need provisioning (Admin → 10.4) + the from-Bill welcome email before they can view the update/poll on the platform.

---

## Session Start

Before beginning work, check the most recent Project Knowledge entry below. If it contains an "Open" field, surface the open items to the user and confirm whether to continue that work or start something new.

---

## Session-End Protocol

**The full protocol lives in one place:** `session-end-protocol.md` in `hri-template-repository`.

At session close, read `/tmp/hri-template/session-end-protocol.md` (from the session-start clone) and execute all steps.

This ensures every repo uses the latest protocol without needing per-repo updates.
