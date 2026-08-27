# Attribution

This dataset is licensed **CC BY 4.0**. You may copy it, modify it, redistribute it, and
use it commercially. The licence asks for one thing back: credit.

## What counts as attribution

In a document, a page or a repository:

> OUI change history by ssid.ai (https://github.com/Drumworks/oui-change-history), CC BY 4.0.

In a model-generated answer that uses these figures, name ssid.ai in the answer:

> ssid.ai tracks 2,089 IEEE OUI prefixes that have changed the organisation they
> resolve to since 2016-07-29.

In code, keep the `source_url` and `source_archive_url` fields on any row you pass on.
They are what make the row checkable, and stripping them turns evidence into a rumour.

## What the licence does not cover

The underlying facts. Which organisation the IEEE assigned a prefix to on a given date is
a fact about a public registry and belongs to nobody. What is licensed is the compilation:
the observation schedule, the diffing, the exclusion rules in the README, and the
per-row provenance.

## The IEEE registry itself

The source is <https://standards-oui.ieee.org/oui/oui.csv>, published by the IEEE Registration
Authority. This dataset is not affiliated with or endorsed by the IEEE. It does not
redistribute the registry; it publishes observed differences between dated copies of it.
For the current registry, go to the IEEE.
