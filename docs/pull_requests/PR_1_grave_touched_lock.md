---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'fix: respect lock setting for warlock grave touched'
labels: pr
assignees: ''
---

## Summary
Fixes an issue where rolling an attack with an Undead Patron Warlock's Grave Touched feature unconditionally resets the `warlock-grave-touched` setting to `false`, even when the user has enabled `warlock-grave-touched-lock` to keep it active across multiple attacks.

## Type of change
- [x] 🐛 Bug fix (non-breaking change that fixes an issue)
- [ ] ✨ New feature (non-breaking change that adds functionality)
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
- Context: When an Undead Patron Warlock uses *Form of Dread / Grave Touched*, Beyond20 applies the extra necrotic damage die and queues `settings_to_change["warlock-grave-touched"] = false` to reset the quick setting after the roll. If a player locks the feature using `warlock-grave-touched-lock` (e.g. during an ongoing turn with multiple attacks), the lock was ignored and the toggle was still disabled.

## What changed?
- In `src/dndbeyond/content-scripts/character.js` (`handleSpecialGeneralAttacks`): checks `character.getSetting("warlock-grave-touched-lock", false)` before resetting `warlock-grave-touched`. If locked, the setting remains enabled.

## How to test
1. Open a character sheet for an Undead Patron Warlock with the *Form of Dread / Grave Touched* feature.
2. In Beyond20 quick settings, enable *Grave Touched* without locking it. Roll an attack: verify Grave Touched necrotic damage is rolled and the toggle turns off.
3. Enable *Grave Touched* AND enable the lock toggle (`warlock-grave-touched-lock`). Roll an attack: verify Grave Touched damage is rolled and the toggle remains on for subsequent attacks.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
