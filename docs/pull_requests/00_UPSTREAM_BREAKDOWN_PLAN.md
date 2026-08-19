# Beyond20 Upstream Pull Request Strategy & Plan

## Overview
This document outlines the breakdown of changes from commits [`0dd6af07`](file:///c:/Users/devli/source/repos/Beyond20) and [`2890bc2d`](file:///c:/Users/devli/source/repos/Beyond20) into **8 independent, atomic Pull Requests** targeted for the upstream [`kakaroto/Beyond20`](https://github.com/kakaroto/Beyond20) repository.

All PR drafts in this folder follow the official [Beyond20 Pull Request Template](file:///c:/Users/devli/source/repos/Beyond20/.github/PULL_REQUEST_TEMPLATE/pull_request.md) with complete descriptions, reproduction steps, testing instructions, and the required AI usage disclosure:
> *"Google's Antigravity CLI tool was used to implement fixes and features specified by Devlin1991"*

---

## PR Breakdown Matrix

| PR # | Branch Name | Title | Type of Change | Base Branch | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PR 1** | `fix/grave-touched-lock` | `fix: respect lock setting for warlock grave touched` | 🐛 Bug fix | `master` | Independent |
| **PR 2** | `fix/item-damage-scraping` | `fix: improve item damage type scraping and fallback for flame tongue` | 🐛 Bug fix | `master` | Independent |
| **PR 3** | `feat/versatile-query-gwf` | `feat: add query option for versatile weapon wielding choice and fix GWF calculation` | ✨ Feature & 🐛 Fix | `master` | Independent |
| **PR 4** | `feat/savage-attacker` | `feat: add support for Savage Attacker feat (2014 and 2024)` | ✨ Feature (Official PHB) | `master` | Independent |
| **PR 5** | `feat/damage-type-breakdown` | `feat: add damage type breakdown to total damage summary` | ✨ Feature (UI / QoL) | `master` | Independent |
| **PR 6** | `feat/exploding-dice-engine` | `feat: add generic exploding dice engine and refactor Sorcerous Burst` | ✨ Feature & ♻️ Refactor | `master` | Independent |
| **PR 7** | `feat/pyromaniac-feat` | `feat: add support for Pyromaniac feat flares` | ✨ Feature (Valda's / Homebrew) | `feat/exploding-dice-engine` | Depends on PR 6 |
| **PR 8** | `feat/blade-cantrips` | `feat: add support for Blade Cantrips (Heroic Sorcery & Melee Cantrips)` | ✨ Feature (Spells) | `feat/exploding-dice-engine` | Depends on PR 6 |

---

## PR Template Files

1. [PR 1: Respect Lock Setting for Warlock Grave Touched](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_1_grave_touched_lock.md)
2. [PR 2: Improve Item Damage Type Scraping and Fallback for Flame Tongue](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_2_item_damage_scraping_fallback.md)
3. [PR 3: Versatile Weapon Query Option & Great Weapon Fighting Fix](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_3_versatile_weapon_query_and_gwf.md)
4. [PR 4: Support for Savage Attacker Feat (2014 and 2024)](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_4_savage_attacker_feat.md)
5. [PR 5: Damage Type Breakdown in Total Damage Summary](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_5_damage_type_breakdown.md)
6. [PR 6: Generic Exploding Dice Engine & Sorcerous Burst Refactor](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_6_exploding_dice_engine.md)
7. [PR 7: Support for Pyromaniac Feat](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_7_pyromaniac_feat.md)
8. [PR 8: Support for Blade Cantrips (Heroic Sorcery & Melee Cantrips)](file:///c:/Users/devli/source/repos/Beyond20/docs/pull_requests/PR_8_blade_cantrips.md)
