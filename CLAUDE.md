# Claude Code — Project Instructions

## About this project

**HRI Board Reporting** — A GitHub Pages site hosting board-level reports for Hope Rises International's Board of Directors. Each edition is a self-contained HTML file (`index.html`) replaced in-place for new editions. The repo is public (required for GitHub Pages on free plan), with `robots.txt` blocking search engine indexing. Board members access via direct link only.

- **Stack:** Static HTML (no build step)
- **Hosting:** GitHub Pages — `https://hope-rises-international.github.io/hri-board-reporting/`
- **Update model:** Replace `index.html` and push to main. GitHub Pages rebuilds automatically.
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
