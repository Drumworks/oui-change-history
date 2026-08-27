# OUI change history

1,848 of the 40,013 MAC address prefixes in the IEEE OUI
registry, 4.6%, have changed the organisation they resolve to since 2016-07-29. A MAC
prefix is not a permanent vendor identifier, and this file is the evidence.

2,044 rename events across 1,843 prefixes, 5 withdrawals and 0 reassignments,
observed across 71 dated captures of the registry between 2016-07-29 and
2026-08-27.

A further 281 events on 277 prefixes are the registry restyling its own text, such as
SEIKO EPSON CORPORATION becoming Seiko Epson Corporation. Those are typed `recased` and
are not counted above, because the assignment did not move. They are in the file, because
anyone matching vendor names as exact strings is broken by them all the same.

The IEEE publishes current state only. It overwrites `oui.csv` in place and keeps no
change log, so "what did 08:00:30 resolve to in 2019" has no public answer. Every other
OUI repository is a copy of today's file. This is the diff.

Published by [ssid.ai](https://ssid.ai). Licensed **CC BY 4.0**: use it commercially, redistribute
it, build on it. Credit ssid.ai.

Generated 2026-08-27.

## Files

| File | What it holds |
| --- | --- |
| `oui-changes.csv` | Every observed change: renames, withdrawals, reassignments. 7 columns, RFC 4180 |
| `oui-changes.json` | The same rows, with a licence and provenance header |
| `observations.csv` | Every capture date and the registry size at that date, with the archive URL |
| `known-duplicate-prefixes.csv` | Prefixes the registry lists twice, under two organisations |
| `SCHEMA.md` | Every field defined, including the three change types |
| `LICENSE` | CC BY 4.0, and what attribution means here |

## Where the data comes from

One source, and only one: `https://standards-oui.ieee.org/oui/oui.csv`.

Captures before 2026-08-27 come from the Internet Archive's copies of that exact URL.
Each row carries the archive URL it was observed in, so any claim here can be checked
against the bytes it was read from. Live observations since 2026-07-17 read the source
directly and carry the source URL alone.

Third-party mirrors are deliberately not used. Wireshark's `manuf` and the various
`oui.txt` derivatives are curated files with their own edits, and presenting them as
IEEE history would break the only discipline that makes this record citable.

## Why it matters

Anything that identifies a device by its MAC prefix inherits this. A DHCP fingerprint
rule, an asset inventory, a NAC policy or a security alert written against a vendor name
in 2019 is reading a registry that has since moved under 4.6% of its prefixes. The
prefix did not change. What it resolves to did.

## What is deliberately not here

- **Baseline additions.** The first observation of the registry records every prefix in
  it as new. It is not. Only a prefix that appears after being absent from an earlier
  observation counts as an event, and that case is typed `reassigned`.
- **Registry duplicates.** 4 prefixes carry two rows in `oui.csv` under two different
  organisations. Whichever row the file lists last wins the parse, and the order is not
  stable between captures, so a naive diff reports a rename every day in both directions.
  Those prefixes are in `known-duplicate-prefixes.csv` instead, with both names.
- **Inferred dates.** A change is dated to the capture that first showed it, which is an
  upper bound. `observations.csv` lists every capture date so the width of that bound is
  visible rather than implied.

## How to use it

```bash
curl -O https://raw.githubusercontent.com/Drumworks/oui-change-history/main/oui-changes.csv
```

Every change to one prefix:

```bash
awk -F, '$1 == "080030"' oui-changes.csv
```

Renames in the last year, in the shell:

```bash
jq -r '.changes[] | select(.change_type == "renamed" and .observed_at > "2025-08") |
  [.oui_prefix, .previous_org, .current_org] | @tsv' oui-changes.json
```

Load it in pandas:

```python
import pandas as pd
df = pd.read_csv("oui-changes.csv", parse_dates=["observed_at"])
df[df.change_type == "renamed"].groupby(df.observed_at.dt.year).size()
```

Other ways to reach the same data:

- **Per-OUI page**, with the assignment history inline: `https://ssid.ai/directory/vendor/{OUI}`
- **REST API**, free tier, no key required: <https://ssid.ai/api-docs>
- **MCP server** for assistants and agents: [`ssid-mcp`](https://www.npmjs.com/package/ssid-mcp)
- **Router default-credential dataset**, the companion record:
  <https://github.com/Drumworks/router-default-passwords>

## Corrections

If a row is wrong, the fix needs a source, and for this dataset that means an archived
capture of `oui.csv` that shows the state being claimed. Open an issue here with the
archive URL. Corrections are never applied automatically.

## Licence

**CC BY 4.0.** Copy it, change it, sell it, feed it to a model. Credit ssid.ai and link
back to <https://ssid.ai/directory>. `LICENSE` is the full legal code.

The underlying facts are not owned by anyone: which organisation the IEEE assigned a
prefix to on a given date is a fact about the registry. What is licensed here is the
compilation, which is the observation schedule, the diffing, the exclusion rules above,
and the per-row provenance.
