# Contributing

The most useful contribution is a **pricing correction**. Pricing in this category moves faster
than features, and a stale number is worse than a missing one.

## Correcting a value

Open a PR editing **both** data files — they must stay in sync:

- `data/crms.csv`
- `data/crms.json`

Then bump `last_verified` for that row to the current `YYYY-MM`.

Include in the PR description a link to the vendor page showing the current value. A screenshot is
fine when the price sits behind a signup wall.

## Adding a tool

A new row needs every column filled, not just a name and a score. Specifically:

- `container_security` and `antidetect_browser` must reflect actual behaviour you have verified,
  not a marketing claim. Vendors describe very different architectures with the same words.
- `price_floor_usd` must come with the right `pricing_unit`. A floor without its unit is not a
  data point.
- `score` should be justified in the PR description against the weighting in
  [`data/criteria.md`](data/criteria.md).

PRs adding a tool with a link and no capability breakdown will be closed.

## Disputing a score

Open an issue rather than a PR. State which criterion you think is misweighted and why. Scores are
judgement calls and the weighting is documented precisely so it can be argued with.

## Validation

Before opening a PR, check that the two data files agree:

```bash
python3 - <<'PY'
import csv, json
csv_rows = list(csv.DictReader(open('data/crms.csv')))
js = json.load(open('data/crms.json'))
assert len(csv_rows) == len(js['tools']), "row count mismatch"
for c, j in zip(csv_rows, js['tools']):
    assert c['tool'] == j['tool'], f"order mismatch: {c['tool']} vs {j['tool']}"
    assert float(c['score']) == j['score'], f"score mismatch on {c['tool']}"
print("OK —", len(csv_rows), "rows in sync")
PY
```
