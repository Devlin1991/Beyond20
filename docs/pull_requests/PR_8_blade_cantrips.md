---
name: Pull Request
about: Create a pull request to help improve the extension
title: 'feat: add support for Blade Cantrips (Heroic Sorcery & Melee Cantrips)'
labels: pr
assignees: ''
---

## Summary
Adds comprehensive support for blade cantrips when attached to weapon attack spell groups on D&D Beyond—including official cantrips (*Green-Flame Blade*, *Booming Blade*, *True Strike*) and *Valda's Spire of Secrets* cantrips (*Burning Blade*, *Arc Blade*, *Frigid Blade*)—handling die increases, attack mode queries, damage conversions, and exploding dice.

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
- Context: When casting blade cantrips embedded within weapon cards on D&D Beyond, Beyond20 previously did not strip duplicate weapon ability modifiers from the spell damage dice, did not include the cantrip name in the attack title, and lacked support for cantrip-specific mechanics like Frigid Blade's die stepping or Arc Blade's ranged attack option.

## What changed?
- **Cantrip Configuration** (`src/dndbeyond/content-scripts/character.js`):
  - Defined `BLADE_CANTRIPS_CONFIG` covering *Burning Blade*, *Arc Blade*, *True Strike*, *Frigid Blade*, *Green-Flame Blade*, and *Booming Blade*.
  - Implemented `handleBladeCantripConversion()`:
    - **Frigid Blade**: Steps up weapon base damage die size ($1d4 \to 1d6 \to 1d8 \to 1d10 \to 1d12 / 2d6$ choice) and converts damage to Cold.
    - **Arc Blade**: Prompts for Melee vs Ranged (15 ft) mode and converts damage to Lightning.
    - **True Strike / GFB / Booming Blade**: Prompts for damage type conversions where applicable.
- **Item Spell Group Parsing** (`src/dndbeyond/content-scripts/character.js`):
  - In `rollItem`: strips weapon ability modifiers from cantrip bonus dice so only the spell damage die is added.
  - Updates roll title to include the spell name (e.g., `Longsword (Green-Flame Blade)`).
- **Renderer Integration** (`src/common/roll_renderer.js`):
  - Added *Burning Blade* exploding dice rule via `ExplodingRule`.
  - Added `BLADE_CANTRIPS_LIST` to `isPrimarySpellOrFeat()` to prevent cantrip damage from being marked conditional.
- **Documentation** (`docs/features.md`): Added `Spells: Blade Cantrips & Heroic Sorcery`.

## How to test
1. On D&D Beyond, attach a blade cantrip to a weapon (e.g., *Green-Flame Blade* or *Burning Blade* on a Longsword).
2. Roll the attack from the weapon card:
   - Verify the roll title displays `Longsword (Green-Flame Blade)`.
   - Verify the bonus spell damage die is added cleanly without duplicate ability modifiers.
3. Test **Arc Blade**: Verify prompt for Melee vs Ranged (15 ft) mode appears and sets range/attack type correctly.
4. Test **Frigid Blade**: Verify weapon die steps up (e.g. 1d8 $\to$ 1d10).
5. Test **Burning Blade**: Verify max die rolls trigger exploding fire dice up to spellcasting modifier.

## Reviewer notes
<!-- Anything you want reviewers to pay attention to: tricky logic, follow-ups, tradeoffs -->
- 
