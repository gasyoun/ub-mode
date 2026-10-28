# BUGHUNT-FINDINGS — ub-mode-install — 28.10.2026

_Created: 28-10-2026 · Last updated: 28-10-2026_

Nightly bug hunt (MG ruling 26-09-2026), repo `ub-mode-install` (origin: https://github.com/unclebob/ub-mode, local clone), 28.10.2026. Hunt scope `src/`, `tools/`, `scripts/`, `plugins/` — **the census found none of these directories exist**; the repo's entire tracked surface is two Markdown files ([README.md](https://github.com/gasyoun/ub-mode/blob/master/README.md), [SKILL.md](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md)). The hunt therefore targeted the operative contract those files define.

**Landing note:** origin is a third-party repo where this account holds read-only access (live `gh api repos/unclebob/ub-mode` → `push: false`). The report lands on the account fork [gasyoun/ub-mode](https://github.com/gasyoun/ub-mode) via worktree branch `bughunt/2026-10-28`, PR + merge there; upstream disposition is routed to MG (residual row below).

## Elapsed

~40 min wall budget; hunt + verification + landing executed in one pass (see final reply for the exact line).

## Ranked findings

### HIGH — none

No HIGH (money/security/data-loss/crash) findings. Evidence: no executable code exists on any ref (census below); history holds exactly one commit ([57a8bdd](https://github.com/unclebob/ub-mode/commit/57a8bdd28e45ed41900ace0bafa2ff76dd9ed70f), 2 files, no deleted blobs — `git log --all --stat`), both files read in full and contain no secret-shaped strings; skill frontmatter parses clean. Phase 3 auto-fix lane: **not triggered** (zero HIGH code bugs; MEDIUM/LOW are report-only per the ruling).

### MEDIUM-1 — Dead provenance references: `poteto-mode` / `pstack` do not exist

[README.md L3](https://github.com/gasyoun/ub-mode/blob/master/README.md#L3) attributes the rules to "portions of @poteto's rules (`poteto-mode` / `pstack`)". Live evidence run 28.10.2026:

- `gh api repos/poteto/poteto-mode` → **404**
- `gh api repos/poteto/pstack` → **404**
- Full enumeration of poteto's public repos (`gh api users/poteto/repos?per_page=100`) → **zero** repos matching `pstack`/`poteto-mode`/`mode` in name; the profile itself exists (`gh api users/poteto` → 200).

The named sources of the whole skill are unverifiable — a broken provenance contract. Nearest live candidate is [poteto/verification-skill-example](https://github.com/poteto/verification-skill-example) (skill-shaped repo in the same account); whether that is the intended referent is an editorial call, parked for MG in the residual row.

### MEDIUM-2 — Dead tool wiring in the operative skill file

[SKILL.md L110](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L110) ("Use the crapper tool"), [L116](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L116) ("Use the dryer tool"), [L120](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L120) ("Use the mutator tool") mandate tool use with **zero pointers** to where those tools live. The documented install copies *only* SKILL.md into the skills directory ([README.md L20](https://github.com/gasyoun/ub-mode/blob/master/README.md#L20)), so the companion-tool URLs that exist in README never reach the executing agent. Live evidence: URL grep over SKILL.md returns 0 matches; README.md carries all 6 tool links. Consequence: the skill's entire "Then Make it Right" phase is unexecutable in a fresh environment — dead config wiring.

### LOW-1 — Textual defects in SKILL.md

- [SKILL.md L4-L5](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L4): frontmatter says "First Make it Work, then **Then** Make it Right" — doubled "Then" (the heading itself is "Then Make it Right", so the phrase garbles the ladder).
- [SKILL.md L111](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L111): `Reduce each function under  CRAP 10` — double space; malformed threshold vs the canonical `CRAP < 10` in [README.md L6](https://github.com/gasyoun/ub-mode/blob/master/README.md#L6) (ambiguous ≤10 vs <10).
- [SKILL.md L116](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md#L116): "eliminate duplication that rank 0.8 or above" — broken verb agreement ("that ranks…"); semantics are consistent with README (eliminate ≥0.8, hold <0.8).

### LOW-2 — Install contract will break if upstream PR #1 merges

Upstream open PR [unclebob/ub-mode#1](https://github.com/unclebob/ub-mode/pull/1) (danihrndzld, "Support installing the skill with npx skills") moves `SKILL.md` to `skills/ub-mode/SKILL.md`. The install instructions at [README.md L20](https://github.com/gasyoun/ub-mode/blob/master/README.md#L20) point at the repo-root file and go stale on merge. Forward-looking note; no action now.

## What verified green

- Skill frontmatter parses: `python3` + `yaml.safe_load` over the `---` block → OK (`name: ub-mode`, description intact).
- All four companion-tool repos live and non-archived (API 200, pushed 2026-10-03…2026-10-21): [unclebob/crapper](https://github.com/unclebob/crapper), [unclebob/dryer](https://github.com/unclebob/dryer), [unclebob/mutator](https://github.com/unclebob/mutator), [unclebob/uml-viewer](https://github.com/unclebob/uml-viewer).
- History census: single commit 57a8bdd, 2 tracked files (`git ls-files`), no removed paths, no secrets; `git ls-remote origin` matches local `master` at 57a8bdd (no drift).
- Fence honored: no `stenogrammy/` surface touched (none exists in this repo).

## Residuals

- GTD @DO (MG): upstream disposition of MEDIUM-1/MEDIUM-2 — PR the fixes to https://github.com/unclebob/ub-mode from the fork, or close; editorial call on the MEDIUM-1 referent (`poteto/verification-skill-example` candidate, unverified).

_Dr. Mārcis Gasūns_
