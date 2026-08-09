# Endgame Analysis — Tiered DIM Wishlist

A DIM wishlist generated from the Endgame Analysis spreadsheet. Every worthwhile roll shows as a thumbs-up in DIM, and each item carries exactly one tier tag describing the best roll it achieves — from "one notable perk" up to "everything the sheet wants, barrel and mag included."

**Why one tag:** DIM applies the *first* wishlist entry that matches an item — that single entry's perks get the thumb badges and its note is what displays and what `wishlistnotes:` filters see. This file is therefore ordered as a strict quality ladder, best tiers first, so an item's one visible tag is always its best tier. A side benefit: the deep entries contain multiple perks per column, so on strong multi-perk drops DIM badges several perks in the same column — something one-perk-per-column wishlists can never show.

**Main file (load in DIM):** `EndgameAnalysis-wishlist-complete.txt`
**Companion (load temporarily for audits):** `vault-audit-legacy-versions.txt`
**Generated:** August 2026, against the live Bungie manifest.

## What's in the main file

| | Count |
|---|---|
| Weapons covered | 748 (all 20 current sheets; `(old)` / `(ignore)` tabs skipped) |
| Weapon-hash variants | 1,344 (Adept / Timelost / reissue hashes included) |
| Total entries | 32,124 |
| Perfect Roll | 2,570 |
| Full Set | 1,344 |
| Combo depth (2+1 … 3+3) | 12,760 |
| God Roll | 10,276 |
| Good Roll | 5,174 (incl. 4 weapons with no barrel/mag columns) |

## The ladder

Each item shows the single tag of the **best** tier it matches, top to bottom:

1. **Perfect Roll** — the drop contains *every* trait the sheet lists for this gun, plus a listed barrel and magazine (+ origin where applicable). The everything-the-sheet-wants gun.
2. **Full Set** — every listed trait, any barrel/mag.
3. **Combo a+b** — the drop holds `a` wishlisted traits in column 3 and `b` in column 4, tagged at the deepest matching depth from 3+3 down to 2+1/1+2. **Reading perfection on multi-perk drops:** compare the depth against the drop's actual option counts — `Combo 3+3` on a 3-and-3 drop means every option is wishlisted.
4. **God Roll** — one wishlisted trait per column plus barrel + mag: the classic single god roll.
5. **Good Roll** — one wishlisted trait per column, any barrel/mag.

A drop below Good Roll — one or more desirable perks but no complete combo — gets **no thumb at all**. That's deliberate: a thumb means "worth a closer look." The companion website still evaluates those drops (shown as "below the bar") so nothing is invisible, it's just not flagged.

Note the tags describe *sheet coverage*: on a weapon where the sheet lists only one perk per column, a drop with both and the right barrel/mag is legitimately "Perfect Roll" — it has everything the sheet wants, even though it isn't a multi-perk drop. Depth information lives in the Combo tags.

## Filtering in DIM

Quotes are required — the phrases contain spaces. None of the tags substring-collide, so each filter is independent.

| Filter | Shows |
|---|---|
| `wishlistnotes:"god roll"` | Perfect single rolls (barrel + mag + traits) |
| `wishlistnotes:"good roll"` | Everything with the right traits |
| `wishlistnotes:"good roll" -wishlistnotes:"god roll"` | Right traits, wrong barrel/mag |
| `wishlistnotes:"combo 2+2"` | Multi-perk drops holding 2+ god rolls (any exact depth works: "combo 3+2", "combo 3+3", …) |
| `wishlistnotes:"combo 3"` | Any depth with 3 wishlisted traits in column 3 |
| `wishlistnotes:"full set"` | Drops containing every listed trait for that gun |
| `wishlistnotes:"notable perk" -wishlistnotes:"good roll"` | Lone desirable perks with no matching partner |
| `wishlistnotes:"tier s"` | Any tier letter from the sheet (s/a/b/…) |

These stack with each other and with any DIM filter: `wishlistnotes:"full set" wishlistnotes:"god roll"` is the jackpot search — everything the sheet wants, barrel and mag included. `is:retiredperk wishlistnotes:"good roll"` finds flagged rolls whose perks can no longer drop (the keep-forever pile).

## The intended workflow

DIM is the **tripwire**, the website is the **reference**. A thumbs-up in DIM means "this drop made a complete combo — check it on the ladder site." The site (`wishlist-ladder-ui.html`) shows the full spreadsheet picture DIM can't: all desirable options per column with icons, the ★ recommended (first-listed) combination, an interactive checker that reproduces DIM's exact tier logic, and drops that fall below the flag bar. Deep links work: `wishlist-ladder-ui.html#w=Mint%20Retrograde` opens straight to a weapon.

## Importing

1. DIM → **Settings** → **Wish List**.
2. Drag `EndgameAnalysis-wishlist-complete.txt` into the file drop area (or paste its contents).
3. Matching items across vault and characters show the green thumbs-up; hover (or tap) to read the tier notes.

Remove any earlier versions of these files — tags and ordering changed (Dual/Triple Roll became Combo 2+2/3+3, and the file order now determines which tag displays, so stale copies actively break the tiering) — the complete file contains everything, and stacking them duplicates notes. It coexists fine with unrelated wishlists like voltron.

## The vault-audit companion

`vault-audit-legacy-versions.txt` is a different kind of list: it flags **all rolls of outdated item hashes** rather than good rolls, so it should be loaded only while auditing, then removed — otherwise every legacy copy carries a permanent thumbs-up and your thumbs stop meaning "good roll."

- **Legacy Version** (398 hashes) — old-era versions of spreadsheet weapons that can't roll the current trait pool at all. Copies you own carry perk combinations that can no longer drop.
- **Old Origin** (216 hashes) — pre-reissue hashes that can roll the current traits but lack the origin trait current drops have.

Filter with `wishlistnotes:"legacy version"` or `wishlistnotes:"old origin"`. These flags mean *outdated*, not *junk* — legacy copies are often the irreplaceable ones. "Legacy" is judged relative to the spreadsheet's listed rolls; for ground-truth per-item retired-perk detection, DIM's native `is:retiredperk` is the authority.

## How weapon variants were handled

Many entries have several in-game versions: Adept / Timelost / Harrowed drops, BRAVE reissues, Rite of the Nine reprisals, Pantheon versions, and same-name weapons from different eras (two entirely different Long Arms, for instance). Rather than matching by name alone, the generator pulled each candidate hash's actual randomized plug sets from the manifest and only emitted entries for hashes that can genuinely roll the listed trait perks. So a "Pantheon version" row automatically landed on the hash with its exclusive perks, every era that can roll a combo gets flagged (old vault copies still light up), and no entry references perks a hash can't roll.

## Origin traits

Origin traits are fixed per item hash — every copy of a hash has its origin(s) — so they can't distinguish a good drop from a bad one. They're appended to entries anyway for parity with common wishlist convention, and only where verified to exist on that exact hash; on older-era hashes where the listed origin predates the reissue, it's omitted rather than baked in as a condition that could never match. Multi-origin weapons use the first listed origin — one is sufficient since all copies carry all of them.

## Known limitations

- **"Contains" is all a wishlist can check.** Matching is positive containment with no negation, so no entry can penalize a drop for an *unlisted* perk. Full Set proves the drop has everything you want; on drops with more options than the sheet lists, the extra slot is simply unknown. A true "every option is wishlisted" indicator would need to be a DIM feature.
- **Masterworks aren't matched.** DIM wishlist matching on masterworks is unreliable; treat the sheet's MW column as guidance for what to masterwork after keeping a roll.
- **Enhanced perks match automatically.** Entries use base perk hashes; DIM normalizes enhanced traits to their base version when matching.
- **All listed perk options rank equally.** The sheet's within-cell ordering (first choice vs. alternative) is flattened.
- **Snapshot in time.** The wishlist reflects the manifest as of generation; regenerate after reissues or perk renames.

## Regenerating

`generate_wishlist.py` (included) produces the complete main file in one run:

```
export BUNGIE_API_KEY=your-key-here
python generate_wishlist.py path/to/spreadsheet.xlsx --ui wishlist-ladder-ui.html
```

The `--ui` flag refreshes the website's embedded weapon/perk data (and entry count) from the same manifest snapshot, so the wishlist and the site can never drift apart.

Requires `pip install requests openpyxl`. It downloads and caches the Bungie manifest (~210 MB, reruns are fast), reads the current-format weapon sheets, and prints per-tier entry counts plus warnings for any weapon it can't match. Its output is byte-identical to the delivered `EndgameAnalysis-wishlist-complete.txt`.
