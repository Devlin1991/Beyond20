---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add query option for versatile weapon wielding choice and fix GWF calculation'
labels: pr
assignees: ''
---

## Summary
Adds an interactive "Query" option to the versatile weapon wielding setting and ensures Great Weapon Fighting (GWF) reroll calculations accurately respect whether the weapon is wielded one-handed or two-handed on each individual roll.

## Type of change
- [x] 🐛 Bug fix (non-breaking change that fixes an issue)
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
- Context: Martial characters often alternate between wielding a versatile weapon (e.g. Longsword or Warhammer) in one hand (with a shield or spellcasting focus) and two hands. Static settings or key modifiers can be cumbersome in fast-paced play.
- Furthermore, `applyGWFIfRequired` was previously evaluated before the wield mode was resolved and checked `character.getSetting("versatile-choice")` directly, potentially applying Great Weapon Fighting rerolls to one-handed versatile strikes erroneously.

## What changed?
- **Settings** (`src/common/settings.js`): Added `"query": "Query 1-Handed / 2-Handed"` choice to `versatile-choice`.
- **Utils** (`src/dndbeyond/base/utils.js`): Updated `applyGWFIfRequired(action_name, properties, damage, versatile_choice = null)` to accept the resolved `versatile_choice` parameter rather than relying solely on the static setting.
- **Character Script** (`src/dndbeyond/content-scripts/character.js`):
  - In `rollItem`: When `versatile_choice === "query"`, prompts the user via `queryGeneric` with options: *One-Handed*, *Two-Handed*, and *Both (Separately)*.
  - Moved `applyGWFIfRequired` execution to occur after the wielding choice is determined and passed the choice directly into it.

## How to test
1. Equip a Longsword (1d8 / 1d10 Versatile) on a character with Great Weapon Fighting fighting style.
2. In Beyond20 Settings, change Versatile weapon choice to `"Query 1-Handed / 2-Handed"`.
3. Roll the Longsword attack:
   - Verify the modal prompt asks *"How are you wielding this weapon?"*.
   - Select **One-Handed**: verify damage rolls `1d8` without Great Weapon Fighting rerolls.
   - Select **Two-Handed**: verify damage rolls `1d10` with Great Weapon Fighting rerolls (`1d10ro<=2`).
   - Select **Both**: verify both `1d8` (1-Hand) and `1d10ro<=2` (2-Hand) roll separately.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
