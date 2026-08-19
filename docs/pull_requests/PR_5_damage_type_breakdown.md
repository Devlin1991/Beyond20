---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add damage type breakdown to total damage summary'
labels: pr
assignees: ''
---

## Summary
Adds an optional damage type breakdown summary (e.g. `Total Combined: 46 (15 Slashing + 31 Fire)`) to roll results when an attack or spell deals multiple types of damage.

## Type of change
- [ ] 🐛 Bug fix (non-breaking change that fixes an issue)
- [x] ✨ New feature (non-breaking change that adds functionality)
- [ ] ♻️ Refactor (no functional change)
- [ ] ⚡ Performance improvement
- [ ] 🧪 Tests (adds or updates tests)
- [ ] 📝 Documentation
- [ ] 🔧 Build/CI
- [ ] 🚨 Breaking change

## ⚠️ AI usage disclosure (required)

> ⚠️ If you used AI tools (e.g., ChatGPT, Copilot, Claude, etc.) to generate or substantially modify **code, tests, documentation, or review comments**, you **must** disclose it here and describe what was AI-assisted.

- [ ] I did **not** use AI for this PR.
- [x] I **did** use AI for this PR (describe below).

Google's Antigravity CLI tool was used to implement fixes and features specified by Devlin1991.

## Motivation & context
- Context: When rolling attacks that deal multiple damage types (such as weapon + Sneak Attack, Divine Smite, or Flame Tongue bonus fire damage), Beyond20 displays individual lines and an aggregate total. DMs and players often need to know the subtotals per damage type to calculate resistances and vulnerabilities quickly.

## What changed?
- **Settings** (`src/common/settings.js`): Added `type-damage-breakdown` setting (`enabled` [default], `disabled`).
- **Roll Renderer** (`src/common/roll_renderer.js`):
  - Aggregates damage totals by type across 1-handed, 2-handed, regular, and critical damage buckets.
  - If more than one damage type exists and the setting is enabled, appends ` (<amount> <Type> + <amount> <Type>)` to the total line.
  - Automatically suppresses the breakdown if only a single damage type is present in the roll.

## How to test
1. Make an attack roll dealing multiple damage types (e.g., Flame Tongue Greatsword, Paladin Divine Smite, or Rogue Sneak Attack).
2. Verify the total damage line displays the combined number followed by the breakdown, e.g. `Total: 28 (18 Slashing + 10 Radiant)`.
3. Make an attack roll with a single damage type (e.g. standard Shortsword): verify no breakdown is appended.
4. Set `type-damage-breakdown` to `disabled` in Beyond20 Options: verify only the numeric total is displayed.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
