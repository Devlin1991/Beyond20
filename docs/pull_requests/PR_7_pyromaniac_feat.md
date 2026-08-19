---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add support for Pyromaniac feat flares'
labels: pr
assignees: ''
---

## Summary
Adds support for the **Pyromaniac** feat (*Valda's Spire of Secrets* / 5e homebrew), automatically rolling fire flares when rolling maximum values on damage dice during fire attacks up to Proficiency Bonus times.

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
- Context: The Pyromaniac feat (from *Valda's Spire of Secrets*) empowers fire-focused characters: whenever the character rolls the maximum value on a damage die in an attack dealing Fire damage, the flames flare, rolling an additional die of damage up to a number of times equal to Proficiency Bonus.

## What changed?
- **Settings** (`src/common/settings.js`): Added `pyromaniac-fire-dice-only` option (defaults to `false` [standard], allowing flares across the attack).
- **Renderer Integration** (`src/common/roll_renderer.js`):
  - Detects if the character has the "Pyromaniac" feat.
  - Registers an `ExplodingRule` for "Pyromaniac Flare" limited by the character's Proficiency Bonus.
  - Normalizes Pyromaniac Flare damage to `Fire` damage.
  - Consolidates multiple flare bursts into a single line (e.g. `Fire (Pyromaniac Flare x2) Damage: 13`) with full hover tooltip breakdown.
  - Added "Pyromaniac" to `isPrimarySpellOrFeat` so flare damage is not treated as conditional.
- **Documentation** (`docs/features.md`): Added `Feat: Pyromaniac`.

## How to test
1. Add the **Pyromaniac** feat to a character sheet on D&D Beyond.
2. Roll a fire spell (e.g. *Fire Bolt*, *Fireball*) or a *Flame Tongue* weapon strike.
3. When any die rolls its maximum face, verify a `Fire (Pyromaniac Flare)` damage die is automatically rolled and included in the output (grouped with `x{count}` if multiple flares trigger).
4. Verify the total number of flares per attack does not exceed the character's Proficiency Bonus.
5. Verify hover tooltip on the flare total shows the individual die rolls.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
