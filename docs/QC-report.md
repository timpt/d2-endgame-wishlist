# Wishlist QC Report — unresolvable perk options

These spreadsheet entries name a perk that does not exist on ANY manifest variant of that weapon.
Likely causes: typo, perk renamed by Bungie, or a perk from a not-yet-released version.
These options were silently skipped during wishlist generation — the rest of the row still generated fine.

| Sheet | Weapon | Column | Listed option |
|---|---|---|---|
| Swords | Sola's Scar | Perk 1 | Sharp Harvest |
## Notes on findings

**Sola's Scar / Sharp Harvest:** the perk *does* exist in the manifest as a trait, but it is not present in any Sola's Scar hash's plug sets in the current manifest — Bungie may have moved it, or it belongs to a version not yet in the plug data. The row's other Perk 1 options generated normally. Worth re-checking after the next manifest update.

Overall: 747 of 748 rows generated with every listed option resolved — the spreadsheet is in excellent shape.
