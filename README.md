# Destiny 2 Endgame Analysis — DIM wishlist + ladder site

A tiered DIM wishlist and a companion web app, both generated from the Endgame
Analysis spreadsheet against the live Bungie manifest.

**In DIM**, a thumbs-up means a drop made a complete combo — worth a look. The
note tells you which tier: Perfect Roll, Full Set, Combo 3+3 down to 2+1, God
Roll, or Good Roll.

**On the site**, you get what DIM can't show: every desirable option per column
with in-game icons, the ★ recommended combination from the spreadsheet, and an
interactive checker that reproduces DIM's tier logic so you can toggle exactly
what your drop rolled.

## Use it

**Wishlist** — DIM → Settings → Wish List → drop in
`EndgameAnalysis-wishlist-complete.txt`, or paste the raw GitHub URL to get
updates automatically (DIM refreshes once a day).

**Site** — open `docs/index.html`, or the GitHub Pages URL once enabled.
Deep links work: `…/index.html#w=Mint%20Retrograde`.

**Filters** (each item carries exactly one tag, so these don't overlap):

| Filter | Best roll is |
|---|---|
| `wishlistnotes:"perfect roll"` | Everything the sheet wants, barrel and mag included |
| `wishlistnotes:"full set"` | Every listed trait, barrel/mag not matched |
| `wishlistnotes:"combo 3+3"` (any depth) | That many wishlisted traits per column |
| `wishlistnotes:"god roll"` | One classic god roll |
| `wishlistnotes:"good roll"` | Right traits, wrong barrel/mag |
| `is:wishlist` | Anything flagged |

Full reference: [`docs/WISHLIST.md`](docs/WISHLIST.md).

## Rebuild after a spreadsheet update

```bash
pip install requests openpyxl
export BUNGIE_API_KEY=your-key     # bungie.net/en/Application
python generate_wishlist.py path/to/spreadsheet.xlsx \
    -o EndgameAnalysis-wishlist-complete.txt \
    --ui docs/index.html
```

One command refreshes both artifacts from the same manifest snapshot, so they
can't drift. Commit the two changed files and push.

## Deploy for a friend

1. The repo lives at [`timpt/d2-endgame-wishlist`](https://github.com/timpt/d2-endgame-wishlist).
   It has to be public for raw wishlist URLs and Pages on a free account.
2. **Site:** Settings → Pages → Deploy from branch `main`, folder `/docs`.
   Lands on `https://timpt.github.io/d2-endgame-wishlist/`.
3. **Wishlist:** open the `.txt` on GitHub → *Raw* → send them that URL to paste
   into DIM's wishlist field.
4. Tell them the model in one line: *a thumb means the drop made a complete
   combo — open the site and toggle what it actually rolled.*

## Also here

- `vault-audit-legacy-versions.txt` — flags outdated item hashes (old perk pools,
  pre-reissue origins). Load it in DIM only while auditing, then remove it:
  it flags whole hashes rather than good rolls.
- [`docs/QC-report.md`](docs/QC-report.md) — spreadsheet options that resolve to
  no manifest perk. Currently one, out of 748 weapons.
