# agents-md-optimizer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[한국어](README.ko.md)

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that optimizes agent context files (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`, etc.) using [Addy Osmani's agents-md methodology](https://addyosmani.com/blog/agents-md/).

## What it does

Applies the **discoverability filter** to your agent context files:

1. **Baseline Analysis** — Classifies every section as `discoverable`, `operational`, or `verbose`
2. **Gotcha Mining** — Scans source code for non-obvious operational knowledge missing from your docs
3. **Optimization** — Removes redundant content, compresses verbose sections, adds discovered gotchas
4. **Verification** — Before/after statistics proving the optimization

Research shows redundant context degrades agent performance by 15-20%, while focused operational knowledge (gotchas, landmines) reduces runtime by ~28%.

## Install

```bash
npx skills add CaesiumY/agents-md-optimizer
```

Or install from a local clone:

```bash
npx skills add ~/path/to/agents-md-optimizer
```

## Usage

After installation, trigger the skill in Claude Code with any of these phrases:

- `optimize CLAUDE.md`
- `streamline CLAUDE.md`
- `agents-md`
- `discoverability filter`
- `add gotchas`
- `optimize AGENTS.md`
- `optimize context file`
- `optimize .cursorrules`

### Flags

| Flag | Effect |
|------|--------|
| `--dry-run` | Analyze and show diff without modifying the file |
| `--report-only` | Output statistics and classification table only |
| `--path <path>` | Target file path (auto-detects if not specified) |
| `--help` | Display usage and exit |

### Auto-Detection

When `--path` is not specified, the skill automatically searches for the first available file:

`CLAUDE.md` → `AGENTS.md` → `.cursorrules` → `CURSOR.md`

### Examples

```
optimize CLAUDE.md
optimize CLAUDE.md --dry-run
optimize CLAUDE.md --report-only
optimize AGENTS.md --path ./AGENTS.md
```

## How it works

### The Discoverability Filter

For each line in your context file, the skill asks:

> "Can an agent discover this using Glob, Grep, or Read within 10 seconds?"

- **Yes** → Remove (directory trees, data flow diagrams, tech stack descriptions)
- **No** → Keep (timing constraints, implicit semantics, platform gotchas)
- **Partially** → Compress to the non-obvious implication

### Gotcha Mining

The skill systematically scans your codebase for 8 categories of hidden operational knowledge:

1. **Timing & Ordering** — Hidden time budgets, sequencing requirements
2. **Implicit Semantics** — Values that mean something unexpected
3. **Platform Detection** — Non-obvious platform behavior
4. **Mandatory Contracts** — Entry points that MUST produce specific output
5. **Error Exit Policies** — Non-standard error handling
6. **Hidden Config Rules** — Configuration behaviors not apparent from schema
7. **Cooldown & Dedup** — Rate limiting with non-obvious scope
8. **Reserved Features** — Defined interfaces with no implementation

### Before/After

A typical optimization on a real project:

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Total lines | 182 | 90 | **-51%** |
| Discoverable lines | 80 | 0 | Removed |
| Operational lines | 60 | 55 | Kept |
| Gotcha items | 0 | 25 | **Added** |

## References

This skill is built on:

- [Addy Osmani — agents-md: the developer's guide to AGENTS.md](https://addyosmani.com/blog/agents-md/)
- [Lulla et al. (2026)](https://arxiv.org/abs/2501.02896) — Human-authored AGENTS.md reduced runtime by 28.64%
- [ETH Zurich](https://arxiv.org/abs/2502.13138) — LLM-generated context files reduced success by 2-3%

## License

MIT
