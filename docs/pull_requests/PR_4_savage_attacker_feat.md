---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add support for Savage Attacker feat (2014 and 2024)'
labels: pr
assignees: ''
---

## Summary
Adds full support for the **Savage Attacker** feat (2014 & 2024 PHB), converting base weapon damage dice to roll with advantage (`2d{faces}kh1`), supporting critical damage calculations on keep-high dice, and providing configurable prompt/auto-application settings.

## Type of change
- [ ] 🐛 Bug fix (non-breaking change that fixes an issue)
- [x] ✨ New feature (non-breaking change that adds functionality)
- [ ] ♻️ Refactor (no functional change)
- [ ] ⚡ Performance improvement
- [ ] 🧪 Tests (adds or updates tests)
- [x] 📝 Documentation
- [ ] 🔧 Build/CI
- [ ] 🚨 Breaking change

## ⚠️ AI usage disclosure (required)

> ⚠️ If you used AI tools (e.g., ChatGPT, Copilot, Claude, etc.) to generate or substantially modify **code, tests, documentation, or review comments**, you **must** disclose it here and describe what was AI-assisted.

- [ ] I did **not** use AI for this PR.
- [x] I **did** use AI for this PR (describe below).

Google's Antigravity CLI tool was used to implement fixes and features specified by Devlin1991.

## Motivation & context
- Context: The Savage Attacker feat (found in both 2014 and 2024 5e core rules) allows a character once per turn when hitting with a weapon attack to roll the weapon's damage dice twice and use the higher roll. Beyond20 lacked dedicated automation for this feat.

## What changed?
- **Settings** (`src/common/settings.js`): Added `savage-attacker` setting (`query` [default], `enabled`, `disabled`).
- **Formula Transformation** (`src/dndbeyond/base/utils.js`): Implemented `applySavageAttackerToFormula()`, converting each weapon damage die (e.g. `1d8` $\to$ `2d8kh1`, `2d6` $\to$ `2d6kh1 + 2d6kh1`).
- **Critical Roll Support** (`src/dndbeyond/base/utils.js`): Updated `damagesToCrits()` regex to capture keep-high/keep-low modifiers (`(?:kh[0-9]+)?(?:kl[0-9]+)?`), ensuring critical damage correctly adds extra dice.
- **Feat Detection & Integration** (`src/dndbeyond/content-scripts/character.js`):
  - In `handleSpecialWeaponAttacks`: checks if character has `"Savage Attacker"` or `"Savage Attacker 2024"`.
  - Only prompts/applies when the character actually has the feat.
  - Replaces weapon base damage formulas with the Savage Attacker formula.
- **Renderer Recognition** (`src/common/roll_renderer.js`): Added `"Savage Attacker"` to `isPrimarySpellOrFeat` so weapon damage dice are not marked conditional.
- **Documentation** (`docs/features.md`): Added `Feat: Savage Attacker (2014 & 2024)`.

## How to test
1. Create or open a character on D&D Beyond with the **Savage Attacker** feat (2014 or 2024).
2. Roll a weapon attack (e.g. Longsword `1d8+3` or Greatsword `2d6+3`).
3. Verify the prompt appears: *"Use Savage Attacker (1/Turn) on this attack?"*.
4. Click **Yes**: verify the roll executes as `2d8kh1+3` (or `2d6kh1 + 2d6kh1 + 3`) and keeps the higher die value.
5. In Beyond20 settings, change Savage Attacker to **Always use** and verify it applies automatically without prompting.
6. On a character **without** the feat, verify no prompt appears and standard damage is rolled.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
