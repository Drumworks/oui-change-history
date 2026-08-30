# Schema

`oui-changes.csv` and the `changes` array in `oui-changes.json` carry the same 7
fields in the same order. CSV is RFC 4180 with LF line endings and a UTF-8 header row.

| Field | Type | Meaning |
| --- | --- | --- |
| `oui_prefix` | string | The 6 hex digits of the OUI, uppercase, no separators. `080030`, not `08:00:30`. |
| `change_type` | enum | One of `renamed`, `withdrawn`, `reassigned`. Defined below. |
| `observed_at` | timestamp | ISO 8601 UTC. The capture that FIRST showed this state, which is an upper bound on when the change happened, not the change's own date. |
| `previous_org` | string, nullable | The organisation the prefix resolved to before this change. Null on a reassignment where the earlier registrant is not in the record. |
| `current_org` | string, nullable | The organisation it resolves to after. Always null on `withdrawn`. |
| `source_url` | string | Always `https://standards-oui.ieee.org/oui/oui.csv`. |
| `source_archive_url` | string, nullable | The Internet Archive capture the observation was read from. Null for live observations, which read the source directly. |

## Change types

| Value | Meaning |
| --- | --- |
| `renamed` | The prefix stayed assigned and the organisation on it changed. Covers renames, mergers and acquisitions. |
| `recased` | The name changed only in capitalisation, punctuation or spacing. Same registrant, restyled by the registry. Excluded from the headline figure and from every "changed hands" count. |
| `withdrawn` | The prefix was present in an earlier capture and absent from this one. |
| `reassigned` | The prefix reappeared after a `withdrawn`. `previous_org` is who held it before the withdrawal where that is known. |

There is no `added` type. Every prefix in the registry appears for the first time in
some capture, and typing that as an event would make the first capture look like 22,000
simultaneous assignments. Only a reappearance after an absence is an event.

## observations.csv

| Field | Type | Meaning |
| --- | --- | --- |
| `observed_at` | timestamp | ISO 8601 UTC of the capture. |
| `oui_count` | integer | How many prefixes the registry held at that capture. |
| `source_url` | string | Always `https://standards-oui.ieee.org/oui/oui.csv`. |
| `source_archive_url` | string, nullable | The Internet Archive capture, where the observation came from one. |

This file is what makes an absence of change provable. A prefix with no row in
`oui-changes.csv` between two dates in this file was observed unchanged across every
capture between them, rather than merely not looked at.

## known-duplicate-prefixes.csv

| Field | Type | Meaning |
| --- | --- | --- |
| `oui_prefix` | string | The prefix. |
| `organisations` | string | Every organisation name observed on it, separated by ` \| `. |
| `observed_flips` | integer | How many times the parsed name changed across captures. |

These prefixes have more than one row in `oui.csv`. The parse keeps the last one, and the
file's row order is not stable between captures, so the name appears to flip. That is a
property of the source file, not a registrant change, so these are kept out of the change
record and published here instead.

Generated 2026-08-30.
