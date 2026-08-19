---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'fix: improve item damage type scraping and fallback for flame tongue'
labels: pr
assignees: ''
---

## Summary
Improves selector resilience when scraping additional damage types from D&D Beyond item cards (supporting `aria-label`, `title`, and modern class names), adds fallback detection for Flame Tongue / ablaze items, and cleans up damage info formatting to prevent `"undefined (info)"` strings.

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
- Context: In recent D&D Beyond sheet markup updates, damage type tooltips on items (e.g., extra fire damage on Flame Tongue or custom magic weapons) are not always captured by `.attr("data-original-title")`. In some cases, the attribute is missing, resulting in `undefined` damage types or missing elemental damage labels.

## What changed?
- In `src/dndbeyond/content-scripts/character.js` (`rollItem`):
  - Broadened damage element selector: `.ct-damage-type-icon .ct-tooltip, .ddbc-damage-type-icon .ddbc-tooltip, [class*='damage-type'], [class*='damageType']`.
  - Added fallback checks against `data-original-title`, `title`, `aria-label`, and text content.
  - Strips trailing `"Damage"` text cleanly.
  - Added fallback to assign `"Fire"` damage type if the item is named "Flame Tongue" or the damage info text contains "ablaze".
  - Cleaned up string formatting when appending `dmg_info` so undefined strings do not appear in the roll label.

## How to test
1. Equip a **Flame Tongue Greatsword** or similar weapon with bonus elemental damage on D&D Beyond.
2. Roll an attack and verify that the bonus `+2d6` damage is labeled as `Fire Damage`.
3. Test a weapon with custom damage info (e.g., `+1d4 (Sneak)` or `+1d6 Radiant`) and verify labels render cleanly without `undefined`.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
