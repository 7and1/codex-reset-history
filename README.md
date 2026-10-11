# Codex Reset History

An independent, event-level dataset of Codex usage-limit reset announcements and banked reset grants since September 17, 2025.

**Canonical dataset, methodology and live updates: [codex-reset.com/timeline](https://codex-reset.com/timeline).**

**Permanent archived snapshot:** [doi:10.5281/zenodo.23289519](https://doi.org/10.5281/zenodo.23289519). The [concept DOI](https://doi.org/10.5281/zenodo.23289518) identifies the archive across versions.

This release is a dated snapshot retrieved on **October 12, 2026 (Asia/Shanghai)**, not an automatically refreshed mirror. It contains **62 events: 52 resets and 10 banked grants**, with announcement dates from September 17, 2025 through October 11, 2026. For current counts and later corrections, use the canonical source.

## Files

| File | Contents |
| --- | --- |
| `codex-resets.csv` | The event table: 62 rows and 8 columns; UTF-8 CSV. |
| `codex-resets.json` | The same events plus metadata, counts, provenance, and download links. |
| `datapackage.json` | Frictionless Data Package schema describing the bundled CSV snapshot. |

## Fields

| Field | Meaning |
| --- | --- |
| `event_id` | Stable string ID: an X post ID, an ID with `-banked` for an additional grant, or a publisher observation ID. Preserve this field as a string. |
| `type` | `reset` or `banked`. |
| `announced_at` | ISO 8601 UTC announcement timestamp. An observed-only event uses its recorded observation time. |
| `date` | UTC calendar date of `announced_at`. |
| `source` | `post` for a public source post, or `observed` for publisher monitoring without a source post. |
| `post_url` | Public source-post URL; empty for an observed-only grant. |
| `banked_state` | `announced`, `arriving`, `available`, or `unknown`; empty for a reset. |
| `summary` | English summary of the event. |

## Sources and inclusion rules

The publisher curates public posts by [Thibault Sottiaux (@thsottiaux)](https://x.com/thsottiaux), with explicitly labeled observations from its monitoring accounts when no public post exists. This snapshot contains 61 source-post rows and one observed-only row.

The archive distinguishes usage-limit resets from banked credits for later use. Each banked grant is counted once. A post announcing both a reset and a banked grant contributes two events. Contextual notes, general promises, individual-account replies, and partial fixes are not intended to count as completed global resets. Banked grant states retain whether a grant was announced, arriving, or available.

According to the publisher's methodology, inclusion combines its alert judgement with manual review. Post timestamps derive from post IDs; monitoring accounts provide additional confirmation where available. Announcement time is not proof of the exact time every account received an event.

The canonical page records an October 10, 2026 reconciliation against other public trackers and explains its corrections. Historical inclusion decisions and completeness remain open to correction.

## Suggested uses and limitations

Use this small curated archive for exploratory analysis of announcement timing, intervals between resets, banked-grant history, and AI software usage-limit policies.

It is not official OpenAI telemetry, individual usage data, a representative sample of all accounts, or a training/evaluation benchmark. It does not establish that every account received an event at the announcement time. Observed-only provenance is weaker than a public source post. The canonical archive can change as events and corrections are incorporated; this snapshot may become outdated. A pattern in past announcements does not guarantee future resets.

## Example

```python
import pandas as pd

events = pd.read_csv("codex-resets.csv", dtype={"event_id": "string"})
events["announced_at"] = pd.to_datetime(events["announced_at"], utc=True)
resets = events.loc[events["type"].eq("reset")].sort_values("announced_at")
gaps_days = resets["announced_at"].diff().dt.total_seconds() / 86400
```

## License and citation

Released by Codex Reset under **[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)**. Provide visible attribution: **Data: [codex-reset.com](https://codex-reset.com/)**. Preserve source links and indicate any changes when reusing the data.

Suggested citation: Codex Reset (2026). *Codex reset history* [dataset]. https://codex-reset.com/timeline

Codex Reset is an independent community project and is not affiliated with OpenAI. “Codex” is a trademark of OpenAI.

## Live source downloads

- [CSV](https://codex-reset.com/data/codex-resets.csv)
- [JSON](https://codex-reset.com/data/codex-resets.json)
- [Data Package](https://codex-reset.com/data/datapackage.json)
- [Timeline API](https://codex-reset.com/api/timeline) — includes contextual timeline material beyond the event-only export.

Questions or corrections: hello@codex-reset.com, or use the contact link on the canonical site.

## Repository snapshot

This GitHub repository contains only the public dataset and its documentation. The CSV is byte-for-byte identical to the canonical export retrieved on October 12, 2026. The Data Package resource path is relative so data tools can open this snapshot locally. The JSON export preserves the publisher’s original metadata and canonical download links.
