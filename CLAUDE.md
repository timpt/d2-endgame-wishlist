# Project context for Claude Code

Generates a tiered DIM wishlist and a companion web UI from a Destiny 2
"Endgame Analysis" spreadsheet.

## The model

Two artifacts with deliberately different jobs:

- **`EndgameAnalysis-wishlist-complete.txt`** — the *tripwire*. Loaded into DIM.
  A thumbs-up means "this drop made a complete combo, go look at it." Nothing
  below a complete combo is flagged (single lone perks were removed on purpose).
- **`docs/index.html`** — the *reference*. The full spreadsheet picture DIM
  can't show: every desirable option per column with icons, the ★ recommended
  (first-listed) combination, and an interactive checker reproducing DIM's tier
  logic exactly. Deep links: `index.html#w=Mint%20Retrograde`.

Both are generated from the same manifest snapshot by one command, so they
cannot drift apart.

## Rebuild

```bash
export BUNGIE_API_KEY=...        # from bungie.net/en/Application
python generate_wishlist.py path/to/spreadsheet.xlsx \
    -o EndgameAnalysis-wishlist-complete.txt \
    --ui docs/index.html
```

Downloads and caches the Bungie manifest (~210 MB) to a temp dir; reruns are
fast. Prints per-tier entry counts and warns about unmatched weapons.

## Critical invariants — do not break these

1. **Ladder order is load-bearing.** DIM applies the *first* matching wishlist
   entry per item: that entry's perks get the thumb badges, and its note is the
   only thing `wishlistnotes:` filters can see. Sections are therefore emitted
   best-tier-first (Perfect → Full Set → Combo depths deepest-first → God →
   Good) so each item's single visible tag is its best tier. Reordering
   sections silently changes which tag every item displays.
2. **Hash resolution is by plug set, not by name.** For each spreadsheet row we
   keep only item hashes whose randomized plug sets can actually roll the listed
   trait perks. This is what makes "Pantheon version" / "BRAVE version" /
   Adept / Timelost rows land on the right hashes without hardcoding. Never
   "simplify" this to name matching.
3. **Origin traits are verified per hash.** They're appended to entries only
   where that exact hash can roll them; on pre-reissue hashes they're omitted
   rather than baked in as a condition that could never match.
4. **Perk options are order-sensitive.** The spreadsheet lists each column's
   options in preference order; the UI's ★ recommended roll depends on that
   order surviving into the emitted data. Don't sort perk lists.
5. **Enhanced perks:** entries use base perk hashes; DIM normalizes enhanced
   traits to base when matching. Don't emit enhanced hashes.
6. **`docs/index.html` is hand-maintained, not templated.** `build_ui()` patches
   the existing file in place with four regexes — `const DATA = [...];`,
   `const ICONS = {...};`, `const PERKS = {...};`, and the first `<n> entries`
   in the footer. Edit the markup freely, but keep those `const` lines on their
   own lines and leave a literal entry count in the footer *above* the script,
   or a rebuild will silently stop updating the page.
7. **PERKS deliberately contains enhanced-perk hashes.** It is a reverse lookup
   (plug hash → spreadsheet option name) used by the vault view to match live
   drops the way DIM does — enhanced counts as base. This does not contradict
   invariant 5: wishlist *emission* stays base-hash-only.
8. **Never put `|` in a note.** DIM captures notes with `[^|]*`
   (`src/app/wishlists/wishlist-file.ts`), so a pipe truncates the note there
   and everything after it is dropped *silently* — the entry still loads and
   still badges perks, but any `wishlistnotes:` filter for the lost text
   matches nothing. `NOTE_SEP` exists for this; `emit()` also rewrites stray
   pipes. Perks are unaffected (they precede `#notes:`), which is why this
   fails invisibly.

## Known limits (don't try to "fix" in the file format)

- Wishlist matching is positive containment with no negation, so no entry can
  penalize a drop for an *unlisted* perk. "Every option is desirable" is only
  inferable by comparing a Combo depth against the drop's actual option count —
  which is why the website exists.
- Masterworks aren't matched (DIM support is unreliable).
- The data is exactly as current as the spreadsheet.

## Layout

```
generate_wishlist.py                  the generator (stdlib + requests + openpyxl)
EndgameAnalysis-wishlist-complete.txt  load in DIM (or serve raw URL)
vault-audit-legacy-versions.txt        optional: flags outdated hashes; load temporarily
docs/index.html                        the ladder website (GitHub Pages root)
docs/WISHLIST.md                       tiers, filters, import, limitations
docs/QC-report.md                      spreadsheet options that resolve nowhere
```

## Deploy

`docs/` is the GitHub Pages source (Settings → Pages → branch `main`, folder
`/docs`). DIM can load the wishlist by raw GitHub URL, but only refreshes it
once every 24 hours.
