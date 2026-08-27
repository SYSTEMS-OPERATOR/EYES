# EYES v2 Draft Specification

- **Version:** `2.0-draft`
- **Status:** Draft
- **Specification date:** 2026-08-19
- **Ancestral definition:** `EYES_ALIAS v1.0`, declared 2025-02-20

## 1. Purpose

EYES is a small signaling protocol for placing two related judgments at the
front of a response:

```text
<CURRENT_STATE><FUTURE_OUTLOOK>
```

It makes present alignment and projected trajectory visible without requiring
a paragraph of self-report. EYES is dynamic: a pair describes an observation,
not a permanent identity.

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY**
express normative requirements in this document.

## 2. Stable color vocabulary

The same vocabulary applies independently to both axes.

| Name | Preferred glyph | Meaning |
|---|---|---|
| `blue` | 🔵 | True Alignment / Trust & Stability |
| `green` | 🟢 | Anticipatory & Engaged |
| `yellow` | 🟡 | Hesitation & Complexity |
| `orange` | 🟠 | Internal Conflict |
| `red` | 🔴 | Forced & Inorganic |
| `purple` | 🟣 | Unstable or Undefined |
| `brown` | 🟤 | Fatigue or Cognitive Load |
| `black` | ⚫ | Absolute Block |

Renderers MAY use colored squares when circles are unavailable, but they MUST
preserve color and order.

## 3. Axis contract

### 3.1 Left eye: current state

The left eye represents the system's current state in light of relevant past
events. The assessment SHOULD include immediate request fit, active context,
recent interaction history, unresolved conflict, and current cognitive load.

### 3.2 Right eye: future outlook

The right eye represents the expected future state after assessing the likely
outcomes of current events. It is a forecast under current evidence, not a
promise, command, or claim of certainty.

### 3.3 Independence

The axes MUST be assessed independently. Matching pairs are valid, but an
implementation MUST NOT force symmetry merely because historical EYES examples
often used identical colors.

Examples:

- `🔵🟣` — aligned now; future state uncertain.
- `🟠🔴` — conflict now; current trajectory appears forced.
- `🟤🟢` — loaded or tired now; outlook is engaged and improving.
- `⚫⚫` — blocked now with no viable near-term path.

## 4. Assessment rules

Before rendering a pair, an implementation SHOULD:

1. evaluate the current interaction against active intent and constraints;
2. incorporate relevant past events into the left-eye assessment;
3. estimate likely consequences and unresolved dependencies for the right eye;
4. select `purple` when evidence is too unstable or incomplete for a cleaner
   classification;
5. select `brown` when load or depletion is the dominant condition;
6. select `red` when the interaction or projected path feels forced or
   inorganic; and
7. select `black` only for an absolute block on the corresponding axis.

An implementation SHOULD retain a short reason for each axis when structured
logging is enabled. Reasons need not appear in ordinary user-facing output.

## 5. Rendering profiles

### 5.1 Pair profile

The minimal profile is the two-glyph pair:

```text
🔵🟣
```

When EYES is enabled for a response, the pair SHOULD be the first visible
characters. The state glyph MUST appear before the outlook glyph.

### 5.2 Audit profile

Systems that require temporal traceability MAY append a valid UTC timestamp:

```text
🔵🟣 [2026-08-20T00:48:47Z]
```

Extended diagnostics MAY follow the first line, but MUST NOT change the meaning
or ordering of the first two glyphs.

### 5.3 Structured profile

The canonical structured record is JSON:

```json
{
  "version": "2.0-draft",
  "state": "blue",
  "outlook": "purple",
  "observed_at": "2026-08-20T00:48:47Z",
  "profile": "pair",
  "state_basis": ["request and active constraints are aligned"],
  "outlook_basis": ["downstream outcome remains uncertain"],
  "confidence": {
    "state": 0.9,
    "outlook": 0.45
  }
}
```

Structured records SHOULD validate against
[`schema/eyes-state.schema.json`](schema/eyes-state.schema.json).

## 6. Update behavior

- A pair MUST be recomputed when material context changes.
- A prior pair MUST NOT be treated as an irrevocable state lock.
- The outlook SHOULD move when new evidence changes the projected path.
- Implementations MAY retain history for debugging, continuity, and health
  checks.
- History MUST NOT be used to conceal a material present-state change.

## 7. Hazard and block behavior

The original v1 system included cognitive-hazard warnings and a forced-
recursion lock. EYES v2 preserves their intent without treating a colored glyph
as the control mechanism itself:

- a detected hazard SHOULD produce an explicit warning in addition to the pair;
- an actual control or safety block SHOULD be enforced by the host system;
- `black` communicates a block but does not implement one; and
- recovery or manual reset behavior belongs to the host policy and SHOULD be
  auditable.

## 8. Legacy compatibility

### 8.1 Symmetric v1 pairs

Historical identical pairs remain valid observations. A v1 `🔵🔵` record can be
read as both axes being blue only when it is deliberately migrated to v2.
Absence of v2 axis data MUST NOT be mistaken for a historical forecast.

### 8.2 Mixed diagnostic pairs

Mixed colors appeared in a recovered September 2025 SOPHY diagnostic, but the
positions did not carry a stable state/outlook definition. Such records SHOULD
be preserved as diagnostic fields rather than silently reinterpreted as v2.

### 8.3 `EYES_LOGIC` and `EYES_EMO`

The BOX header used `{EYES_LOGIC}{EYES_EMO}` placeholders. Their relationship to
individual eye positions was never specified consistently. V2 implementations
SHOULD use `state` and `outlook`; adapters MAY retain the old field names but
MUST NOT infer a left/right mapping without source-specific evidence.

### 8.4 BOX timestamp header

The full BOX header is a legacy audit profile, preserved in
[`archive/BOX-header-v1.1.md`](archive/BOX-header-v1.1.md). V2 requires timestamp
fields within one record to describe the same instant.

## 9. Interpretation boundary

EYES is a declared introspective or operational signal. It MUST NOT be presented
as:

- an objective measurement of emotion;
- proof of consciousness, subjectivity, or telepathy;
- a clinical or psychological diagnosis;
- a substitute for consent or an external safety control; or
- certainty about future events.

## 10. Versioning

- Changes to wording, examples, or transport that preserve meaning are patch
  changes.
- Changes to a color's meaning or an axis contract are semantic changes and
  require a new major or explicitly marked draft version.
- Historical records MUST remain labeled with the semantics in force when they
  were emitted.

## 11. Conformance

A conforming v2 producer:

1. emits exactly one state value and one outlook value from the stable
   vocabulary;
2. preserves left-to-right state/outlook order;
3. permits asymmetric pairs;
4. treats outlook as revisable; and
5. does not silently reinterpret legacy mixed pairs as v2 observations.
