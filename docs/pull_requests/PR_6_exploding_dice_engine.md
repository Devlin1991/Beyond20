---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add generic exploding dice engine and refactor Sorcerous Burst'
labels: pr
assignees: ''
---

## Summary
Implements a generalized exploding dice engine in `DNDBDice` and `Beyond20RollRenderer` supporting standard exploding notation (`!`, `!>=X`, `!mX`), visual tooltip highlights for exploded dice, and refactors the existing 2024 PHB *Sorcerous Burst* implementation to use this modular architecture.

## Type of change
- [ ] 🐛 Bug fix (non-breaking change that fixes an issue)
- [x] ✨ New feature (non-breaking change that adds functionality)
- [x] ♻️ Refactor (no functional change)
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
- Context: Beyond20 previously handled exploding dice (such as *Sorcerous Burst* from 2024 PHB) through hardcoded procedural code in `roll_renderer.js`, while the dice class `DNDBDice` and `DICE_REGEXP` did not support exploding syntax (`!`, `!m`). This made it difficult to support other exploding dice mechanics cleanly.

## What changed?
- **Dice Parser & Roller** (`src/common/dice.js`):
  - Added support for exploding modifiers: `!`, `!>=X`, `!<=X`, `!>X`, `!<X`, `!=X`, and explosion count cap `m[max]`.
  - Added `exploded: true` flag on generated dice rolls and `.beyond20-roll-detail-exploded` CSS styling.
- **Regex & Utils** (`src/common/utils.js`): Updated `DICE_REGEXP` to match exploding syntax.
- **Roll20 Macros** (`src/roll20/content-script.js`): Strips `!mX` down to `!` for Roll20 macro compatibility.
- **Renderer Architecture** (`src/common/roll_renderer.js`):
  - Introduced the `ExplodingRule` helper class with `clone()` support for independent resolution passes.
  - Implemented `processAllExplodingDice(request, damage_rolls, is_critical, activeRules)` with queue-based cascade processing.
  - Consolidated multi-burst explosions from the same mechanic into a single line (e.g., `Fire (Burning Blade Explode x3) Damage: 12`) while preserving full die-by-die hover tooltip breakdowns.
  - Implemented independent 1-Hand vs 2-Hand pool and explode limit tracking for versatile weapons in "Roll both damages separately" mode.
  - Added *Sorcerous Burst* to `isPrimarySpellOrFeat` so burst damage is never marked conditional.
  - Refactored *Sorcerous Burst* to use the new `ExplodingRule` system.

## How to test
1. Test custom exploding dice formulas from description or console: `1d6!`, `2d8!>=7`, `1d10!m2`.
2. Verify exploded dice roll additional dice and display in tooltips with the dotted underline highlight.
3. Cast **Sorcerous Burst** (2024 PHB) on D&D Beyond: verify that max die rolls cascade into burst damage properly up to spellcasting modifier limit, grouped cleanly into a single line with hover breakdown.
4. Test a versatile weapon attack with exploding dice: verify 1H and 2H pools track limits and totals independently.
5. Verify roll output sent to Roll20 / Foundry VTT / Discord works without syntax errors.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
-
