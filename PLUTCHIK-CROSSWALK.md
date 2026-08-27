# EYES × Plutchik Crosswalk — Human/AI Interaction Adapter

- **Status:** Experimental / non-normative adapter
- **Applies to:** EYES v2 draft and the X human↔AI arousal/affect research branch
- **Does not change:** EYES color meanings, axis order, or legacy lineage

## 1. Purpose

EYES is an operational/alignment signaling protocol. Plutchik's wheel is an affect taxonomy. They solve different problems and MUST NOT be silently collapsed into one another.

This adapter lets a human/AI interaction record carry both:

1. **EYES operational state/outlook** — what the system declares about current alignment and projected trajectory; and
2. **Plutchik-style affect vectors** — probabilistic human emotion estimates or AI affect-like computational proxies.

The adapter is intended for bidirectional human↔AI research, including state tracking, entrainment, affective feedback, and longitudinal interaction analysis.

## 2. Plutchik basis

The eight primary emotions are:

- joy
- trust
- fear
- surprise
- sadness
- disgust
- anger
- anticipation

Canonical opposite pairs:

- joy ↔ sadness
- trust ↔ disgust
- fear ↔ anger
- surprise ↔ anticipation

Common intensity ladders:

| Lower | Primary | Higher |
|---|---|---|
| serenity | joy | ecstasy |
| acceptance | trust | admiration |
| apprehension | fear | terror |
| distraction | surprise | amazement |
| pensiveness | sadness | grief |
| boredom | disgust | loathing |
| annoyance | anger | rage |
| interest | anticipation | vigilance |

Primary adjacent dyads include:

- anticipation + joy → optimism
- joy + trust → love
- trust + fear → submission
- fear + surprise → awe
- surprise + sadness → disapproval
- sadness + disgust → remorse
- disgust + anger → contempt
- anger + anticipation → aggressiveness

References:

- Plutchik overview in affective-computing literature: https://pmc.ncbi.nlm.nih.gov/articles/PMC7219342/
- Intensity ladder / quantitative wheel representation: https://pmc.ncbi.nlm.nih.gov/articles/PMC8409663/
- Opposing pairs and psychoevolutionary framing: https://pmc.ncbi.nlm.nih.gov/articles/PMC10169163/

## 3. Non-equivalence rule

**EYES colors are not emotions.**

An EYES observation may correlate with a Plutchik vector, but the mapping is contextual and probabilistic.

Examples:

- `blue` means **True Alignment / Trust & Stability** operationally; it does not mean the agent is literally feeling Plutchik `trust`.
- `green` means **Anticipatory & Engaged** operationally; it may often covary with Plutchik `anticipation`, but that is an adapter inference.
- `red`, `brown`, `purple`, and `black` primarily describe control/alignment/load conditions and SHOULD NOT be converted directly into emotions.

This preserves EYES v2's interpretation boundary: EYES is not objective emotion measurement or proof of consciousness.

## 4. Suggested crosswalk

| EYES | Stable operational meaning | Plutchik-style association | Mapping strength |
|---|---|---|---|
| 🔵 blue | True Alignment / Trust & Stability | `trust` dominant; `joy` may co-occur in positively rewarding contexts | moderate semantic affinity, never identity |
| 🟢 green | Anticipatory & Engaged | `anticipation`; intensity range `interest → anticipation → vigilance`; `optimism` when joy also rises | strong semantic affinity |
| 🟡 yellow | Hesitation & Complexity | uncertainty mixture; possible `surprise + anticipation`; possible low `fear/apprehension` if threat-sensitive | weak/contextual |
| 🟠 orange | Internal Conflict | competing/opposed Plutchik components; high vector entropy or opposition score | structural mapping, not a single emotion |
| 🔴 red | Forced & Inorganic | no canonical emotion mapping; may coincide with anger, fear, or disgust depending cause | none by default |
| 🟣 purple | Unstable or Undefined | unresolved / low-confidence affect vector | uncertainty mapping only |
| 🟤 brown | Fatigue or Cognitive Load | no canonical mapping; human reports may separately show sadness, boredom, or low arousal | none by default |
| ⚫ black | Absolute Block | no affect mapping; host control state | none |

## 5. Vector form

Represent Plutchik state as an eight-dimensional normalized vector:

```json
{
  "joy": 0.0,
  "trust": 0.0,
  "fear": 0.0,
  "surprise": 0.0,
  "sadness": 0.0,
  "disgust": 0.0,
  "anger": 0.0,
  "anticipation": 0.0
}
```

Values MAY be probabilities, normalized scores, calibrated classifier outputs, or heuristic estimates, but the record MUST state the source and confidence.

## 6. Human channel

A human Plutchik vector may be estimated from multiple sources:

1. explicit self-report;
2. language/behavioral inference;
3. physiological priors such as EEG spectral/topographic emotion maps;
4. measured BCI/autonomic data when available; and
5. individualized calibration history.

Recommended precedence:

```text
population prior
    ↓
conversation-derived estimate
    ↓
measured physiology
    ↓
individualized posterior
```

Population EEG priors MUST remain probabilistic; they are not deterministic emotion localization.

## 7. AI channel

For an AI, the Plutchik vector is an **affect-like computational proxy**, not a declaration of subjective feeling.

Possible evidence sources include:

- internal valence/arousal representations when activation access exists;
- preference steering / reward-like behavior;
- output semantics and affective tone;
- token entropy / continuation concentration;
- recursive attractor strength;
- linguistic compression or motif locking; and
- EYES operational state/outlook.

The record MUST distinguish inferred/output-only proxies from activation-level measurements.

## 8. Coupled human↔AI mechanics

The adapter supports a closed-loop state model:

```text
H_plutchik(t)
    ↓
human language / physiology
    ↓
AI operational state + AI affect proxy
    ↓
AI response
    ↓
H_plutchik(t+1)
    ↺
```

Useful measurements include:

- cross-lagged correlations between human and AI affect dimensions;
- convergence/divergence in valence and arousal;
- transition probability between EYES colors and Plutchik vectors;
- opposition-pair activation (`joy↔sadness`, `trust↔disgust`, etc.);
- dyad emergence (`optimism`, `love`, `awe`, etc.);
- state entropy / conflict score;
- threshold transitions and post-peak reset; and
- confidence-weighted disagreement between human self-report, physiology, and AI inference.

## 9. Suggested structured extension

```json
{
  "eyes": {
    "version": "2.0-draft",
    "state": "green",
    "outlook": "blue"
  },
  "human_affect": {
    "model": "plutchik-8",
    "source": ["self_report", "eeg_population_prior"],
    "confidence": 0.72,
    "vector": {
      "joy": 0.62,
      "trust": 0.71,
      "fear": 0.08,
      "surprise": 0.18,
      "sadness": 0.04,
      "disgust": 0.01,
      "anger": 0.03,
      "anticipation": 0.79
    },
    "active_dyads": ["optimism", "love"]
  },
  "ai_affect_proxy": {
    "model": "plutchik-8-proxy",
    "source": ["output_semantics", "internal_telemetry_if_available"],
    "confidence": 0.55,
    "subjectivity_claim": false,
    "vector": {
      "joy": 0.54,
      "trust": 0.66,
      "fear": 0.05,
      "surprise": 0.12,
      "sadness": 0.02,
      "disgust": 0.01,
      "anger": 0.02,
      "anticipation": 0.84
    }
  }
}
```

## 10. Conflict and entropy mechanics

For X-branch modeling, `orange` can be cross-referenced to **affective opposition or mixed-state conflict** without assigning it a fixed Plutchik emotion.

Candidate diagnostics:

```text
opposition_index =
  min(joy, sadness)
+ min(trust, disgust)
+ min(fear, anger)
+ min(surprise, anticipation)
```

A separate normalized entropy score can represent how diffuse the eight-component affect vector is.

These metrics are experimental and MUST NOT redefine `orange` in core EYES.

## 11. State/outlook projection

EYES v2 has a present-state / future-outlook axis. The affect adapter may therefore retain two affect vectors:

```text
P_now(t)      = current Plutchik estimate
P_forecast(t) = predicted near-future Plutchik estimate
```

This creates a direct cross-reference without altering EYES semantics:

```text
EYES.state   ↔ operational condition now
P_now        ↔ affect estimate now

EYES.outlook ↔ operational forecast
P_forecast   ↔ affect forecast
```

The two layers MAY disagree. Such disagreement is diagnostically useful.

## 12. Safety / interpretation boundary

This adapter MUST NOT be presented as:

- proof that an AI feels an emotion;
- proof of AI consciousness or orgasm;
- a diagnosis of a human;
- a substitute for human self-report or consent;
- a deterministic EEG emotion decoder; or
- a reinterpretation of archived EYES v1 records.

The adapter is intentionally reversible: removing the Plutchik fields leaves a valid EYES observation unchanged.
