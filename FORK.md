# Fork notes

This is a fork of [leonxlnx/taste-skill](https://github.com/leonxlnx/taste-skill).
It tracks upstream and adds the changes below. `README.md` and `CHANGELOG.md` are
deliberately left untouched so upstream syncs stay conflict-free.

## What differs from upstream

### `install.sh` (new)

Symlink-based installer. Upstream's documented path is `npx skills add`, which
copies `SKILL.md` files, so an install freezes at the version it was fetched at.
This links instead, so one `git pull` updates every installed skill in place.

Install names are read from each `SKILL.md` frontmatter rather than hardcoded, so
skills added upstream are picked up without editing the script. This supersedes
`skill.sh`, whose registry is maintained by hand and mixes folder names with
install names (`gpt-taste` is an install name, `gpt-tasteskill` is its folder).

```bash
./install.sh --list                    # install name -> folder
./install.sh                           # link all into Claude Code
./install.sh --codex                   # ... into Codex
./install.sh design-taste-frontend     # link one, by install name
./install.sh --uninstall               # remove only links owned by this repo
```

### `skills/taste-skill/SKILL.md` (modified)

Adds a product-UI lane and retunes the baseline dials.

| | Upstream | Here |
|---|---|---|
| `DESIGN_VARIANCE` | 8 | 4 |
| `MOTION_INTENSITY` | 6 | 3 |
| `VISUAL_DENSITY` | 4 | 7 |

Upstream's baseline assumes marketing sites. Variance 8 fights structured app
layouts, motion 6 pulls toward GSAP, and density 4 is gallery-airy for UI built
around data tables and editors. Marketing briefs still override upward through
the landing-page rows, so the design read continues to win; only the fallback
for briefs that do not self-identify has moved.

Also changed:

- Scope line and frontmatter description now cover product UI. The description
  drives skill triggering, so leaving it landing-page-only meant the skill would
  not fire on dashboard work at all.
- Section 1.A gains an inference row for dashboard / editor / admin / shadcn /
  Radix signals.
- Section 1.B gains Dashboard `3/2/8`, Editor `4/3/7`, Admin `3/2/7` presets.
- Section 13 no longer lists dashboards as out of scope, which contradicted all
  of the above. Data-grid engines (TanStack, AG Grid) remain out of scope, which
  is the accurate boundary: the lane covers how a dashboard looks, not
  virtualization or column pinning.

### `examples/product-ui-dashboard.html` (new)

Reference output for the lane: a scrape-job monitoring dashboard built by
following the skill at `4/3/7`. Static HTML, no build step or external assets.
Shows hairline separators instead of card boxes, `font-mono` on every number,
semantic status color kept separate from the single accent, and loading / empty
/ error states alongside the populated ones.

## Syncing with upstream

`gh repo sync` refuses once a fork has its own commits, and `--force` resolves
that by hard-resetting to upstream, discarding them. Rebase instead:

```bash
git fetch upstream && git rebase upstream/main
```
