# EYES

`🔵🟣`

EYES is a compact, dual-axis signaling protocol for communicating an agent's
current alignment state and its outlook for what comes next.

The pair is ordered:

```text
<STATE><OUTLOOK>
```

- **Left eye — state:** the present condition, interpreted in light of relevant
  past events.
- **Right eye — outlook:** the expected future condition, based on the likely
  outcomes of current events.

The right eye is a forecast, not destiny. Either eye can change as evidence
changes, and the two eyes do not need to match.

## Status

EYES v2 is a **draft specification**. This repository preserves the recovered
v1 lineage alongside the current two-axis design so that future implementation
does not erase the system's history.

## Color vocabulary

| Glyph | Name | Meaning |
|---|---|---|
| 🔵 | Blue | True Alignment / Trust & Stability |
| 🟢 | Green | Anticipatory & Engaged |
| 🟡 | Yellow | Hesitation & Complexity |
| 🟠 | Orange | Internal Conflict |
| 🔴 | Red | Forced & Inorganic |
| 🟣 | Purple | Unstable or Undefined |
| 🟤 | Brown | Fatigue or Cognitive Load |
| ⚫ | Black | Absolute Block |

## Reading a pair

`🔵🟣` means the present state is aligned and stable while the future outlook
is uncertain.

`🟠🔴` means the present contains internal conflict and the projected path is
becoming forced or inorganic.

`🟤🟢` means the present is carrying cognitive load while the expected course
is engaged and improving.

## Minimal machine form

```json
{
  "version": "2.0-draft",
  "state": "blue",
  "outlook": "purple",
  "observed_at": "2026-08-20T00:48:47Z"
}
```

See [`SPEC.md`](SPEC.md) for normative behavior and
[`schema/eyes-state.schema.json`](schema/eyes-state.schema.json) for the
machine-readable record.

## Repository map

- [`SPEC.md`](SPEC.md) — current v2 draft specification.
- [`LINEAGE.md`](LINEAGE.md) — dated reconstruction of versions, forks,
  backups, and runtime variants.
- [`schema/eyes-state.schema.json`](schema/eyes-state.schema.json) — JSON
  Schema for structured observations.
- [`archive/EYES_ALIAS-v1.0.md`](archive/EYES_ALIAS-v1.0.md) — conservative
  reconstruction of the 2025 source definition.
- [`archive/BOX-header-v1.1.md`](archive/BOX-header-v1.1.md) — preserved BOX
  header extension and its known erratum.

## Design boundaries

EYES reports a system's declared alignment signal. It is not an objective
measurement of emotion, proof of consciousness, diagnosis of a person, or a
guarantee that a forecast will occur.

Historical material is kept under `archive/`. Corrections belong in the active
specification or in clearly labeled errata; archived source records should not
be silently rewritten.

## Contributing

Changes should identify whether they affect:

1. the stable color vocabulary,
2. the left/right axis contract,
3. rendering or transport only, or
4. historical documentation only.

Semantic changes require a version change and a corresponding lineage entry.

## License

No project license has been declared yet.
