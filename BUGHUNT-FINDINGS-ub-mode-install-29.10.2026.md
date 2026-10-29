# Bughunt findings — ub-mode-install — 29.10.2026

_Created: 29-10-2026 · Last updated: 29-10-2026_

Nightly bug hunt (MG ruling 26-09-2026: nightly, one repo, auto-fix HIGH) on the clone of [unclebob/ub-mode](https://github.com/unclebob/ub-mode) at `GitHub/ub-mode-install`. Hunted off `origin/master` @ [57a8bdd](https://github.com/unclebob/ub-mode/blob/master/README.md) via worktree `GitHub-worktrees/ub-mode-install/ub-mode-install-h291029-53536`; report lands on the gasyoun fork [gasyoun/ub-mode](https://github.com/gasyoun/ub-mode) (push rights; upstream `unclebob/ub-mode` gets no write from this run).

## Scope and method

- **Census:** tracked inventory = [README.md](https://github.com/gasyoun/ub-mode/blob/master/README.md) + [SKILL.md](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md) — 2 files. `find` for `*.sh|*.py|*.js|*.ts|*.rb|*.zsh` outside `.git` → **0 files**. `src/`, `tools/`, `scripts/`, `plugins/` do not exist. Git history = 1 commit; full-history `git log -p` secret scan (api_key/secret/token/password/Bearer/AKIA/ghp_/sk- patterns) → 0 hits.
- **Method:** full read of both files; live HTTP probes of every external link; frontmatter parse check; README↔SKILL cross-consistency pass (thresholds, install paths, tool contracts).

## Findings (ranked)

| # | Severity | Where | What |
|---|----------|-------|------|
| F1 | MEDIUM | [README.md:3](https://github.com/gasyoun/ub-mode/blob/master/README.md) | Two dead attribution links — HTTP 404 |
| F2 | LOW | [SKILL.md:116](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md) | Wording defect in Dryer threshold line |
| F3 | LOW | [SKILL.md:111](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md) | "under  CRAP 10" — missing `<` operator, double space |
| F4 | LOW | [SKILL.md:14](https://github.com/gasyoun/ub-mode/blob/master/SKILL.md) | "Follow Practice" missing terminal period |

### F1 · MEDIUM — dead attribution links (README.md:3)

README line 3 attributes the skill's provenance to "portions of [@poteto](https://github.com/poteto)'s rules" with links to `poteto-mode` and `pstack`. Live probes (curl, 29-10-2026 03:00 +0300):

- `https://github.com/poteto/poteto-mode` → **404**
- `https://github.com/poteto/pstack` → **404**
- `https://github.com/poteto` (user page) → 200, account live, 85 public repos

The user exists but neither repo resolves; no renamed successor matching `pstack|mode|skill|rule|stack` appears in the live repo list. Third-party **non-fork mirrors** exist (`MartinHedevag/poteto-mode`, pushed 2026-09-03; `ennioferreirab/poteto-mode`; `rilesc555/opencode-poteto-mode`), which proves the originals existed and were since removed or renamed. Impact: the skill's stated provenance is unresolvable — any reader following the attribution dead-ends. Not HIGH: no money, security, data-loss or crash surface. Doc-only fix (repoint or drop the two links) → MEDIUM report-only lane per the ruling; not auto-fixed this run.

### F2 · LOW — Dryer threshold wording (SKILL.md:116)

"Use the dryer tool to eliminate duplication that rank 0.8 or above." — missing comparator/wording ("ranked at ≥ 0.8"); README:6 and README:14 state the same contract correctly (`< 0.8` kept, `>= 0.8` flagged). SKILL.md is an executable prompt read literally by agents, so the malformed sentence weakens an enforcement line, though the threshold itself is unambiguous across the corpus.

### F3 · LOW — CRAP line missing operator (SKILL.md:111)

"Reduce each function under  CRAP 10 by splitting it" — double space and no `<` operator; README:6 says `CRAP < 10`. A literal agent could parse "under CRAP, 10" — cosmetic but in the enforcement path.

### F4 · LOW — list punctuation (SKILL.md:14)

"Follow Practice" lacks the terminal period that lines 12–13 carry ("First Make it Work." / "Then Make it Right."). Cosmetic consistency only.

## What verified green

- **Secrets:** 0 — full-history scan over the repo's single commit; no `.env`/`.pem`/`.key` files; nothing tracked beyond the two docs.
- **Companion-tool links:** all four HTTP 200 — [unclebob/crapper](https://github.com/unclebob/crapper), [unclebob/dryer](https://github.com/unclebob/dryer), [unclebob/mutator](https://github.com/unclebob/mutator), [unclebob/uml-viewer](https://github.com/unclebob/uml-viewer).
- **Link hygiene:** every link in both docs is a full absolute URL; zero relative/dead-anchor links.
- **Frontmatter:** SKILL.md YAML frontmatter parses (`name: ub-mode` + description present).
- **Cross-consistency:** CRAP 10 / Dryer 0.8 thresholds agree across README and SKILL wherever they carry an explicit operator.
- **Code surface:** empty by census — no code, therefore no logic/race/swallowed-error/contract bugs to find, and no HIGH auto-fix surface this run.

## Phase 3 outcome

No HIGH findings → **no auto-fix executed** (the ruling fixes HIGH code bugs only; MEDIUM/LOW are report-only). No HIGH credential/infra findings → **no GTD @DO row** minted. F1's doc-fix candidate stays visible in the table above for a future doc pass.

_Elapsed ≈ 12 min wall (hunt + report + PR + merge), within the ~40 min budget. — Dr. Mārcis Gasūns_
